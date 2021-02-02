---
layout: post
title: "[디자인패턴] Singleton Pattern"
subtitle: "Java로 싱글톤 패턴 공부해보기"
background: '/img/bg_technology.jpg'
categories: technology

---

### 0. Design Pattern

- 디자인 패턴이란? Software development 에서 반복적으로 발생하는 **공통의 issue**를 해결하기 위해 연구된, **특정 문제해결을 위한 전형적인 방법.**

- '이러이러한 문제 상황'에서는 '이 방법으로 해결하는 게 좋더라' - 일종의 축적된 Solution.

- 디자인 패턴 이용의 장점

  - 반복되어 발생하는 문제, 이슈를 효과적으로 해결할 수 있는 방법을 제시함.
  - 디자인 패턴에 익숙한 다수의 개발자들이 설계 구조를 쉽게 인지 가능 - 유지보수가 용이함

- 디자인 패턴의 종류 : Singleton, Decorator, Observer, Strategy... 등 수십개 존재함.

- 이 중 Singleton Pattern 은 **creational design patterns** 카테고리에 속함.

  

---

### 1. Singleton Pattern 이란?

- **Instance가 메모리상에 <u>오직 1개만 생성</u>되도록 설계하는 것.**

  ##### [ Class / Object / Instance ] 

  ```java
  class Car { // 클래스
      int number;
      String kind;
      
      public Car(int number, String kind) {
  		...
      }
  }
  
  public class Test {
      public static void main(String[] args) {
          Car car; // 객체 선언
          car = new Car(1, "Avante"); // 인스턴스 - 실제 메모리에 할당
      }
  }
  ```

  - **Class** : 도면, 디자인, 설계도 - '자동차'가 어떤 **속성(field)**을 가지고 어떻게 **작동(method)**할까?
  - **Object** : 실체가 있으나 명확한 구분점이 없는 것.
  - **Instance** : 실체가 있으며 다른 인스턴스와 명확히 구분 가능함 (unit).
    - JVM은 instance를 위한 메모리 공간을 할당한다.
    - 

---

### 2. Singleton Pattern은 왜, 그리고 언제 쓰나?

- 왜 하나만 생성되어야 하나?
  - 공통 기능을 가진 instance를 여러번 생성하면 그만큼 메모리공간이 낭비되므로.
- 언제 쓰나?
  - Logging 객체를 쓸 때 : 로그를 찍는 것은 어플리케이션 전반에 걸쳐서 계속 이루어져야 하는 업무이므로, 만약 log 인스턴스를 계속 생산하면 리소스가 낭비됨.
  - DB연결 객체를 쓸 때 : 마찬가지...
  - 그 외 레지스트리 설정 처리 객체, 스레드 풀, 캐시 등..



---

### 3. Singleton Pattern 구현 예시 (늦은 초기화 Lazy Initialization)

```java
public class Singleton {
    private static Singleton instance; // 최초 instance는 null

    private Singleton() { // private 생성자로 new 인스턴스 생성을 막는다
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // 메소드 최초 실행 시 인스턴스 초기화
        }
        return instance;
    }
}

public class Practice {
    public static void main(String[] args) {
        /* 싱글톤 인스턴스 생성 */
        Singleton singleton1 = new Singleton(); // inaccessible
        Singleton singleton2 = Singleton.getInstance(); // accessible
        Singleton singleton3 = Singleton.getInstance(); // accessible

        /* 주소값 출력 */
        System.out.println(singleton2);
        System.out.println(singleton3);
    }
}

/* 출력 주소값 */
Singleton@10f87f48
Singleton@10f87f48

```



---

### 4. Singleton Pattern의 구현에 실패하는 경우 - Multi Thread 환경일 때

- 만약 여러 스레드가 이 객체가 생성되기 전에 동시에 접근한다면 경합 조건(Race Condition)이 발생할 수 있다.

- 객체를 만들기전에 스레드가 동시에 접근하면 객체를 여러개 만들게되는 상황에 직면할 수 있다.

- 이에 대한 해결방법은**동기화**를 적용시키는 것이다.
  - 해결방법 1 : **synchronized** 키워드 이용하여 synchronized 함수로 만들기
    - **synchronized** 는 해당 객체에 lock을 거는 것과 같다. 즉, 하나의 스레드 작업이 완료되기 전까지 다른 스레드가 접근을 못하게 방지하는 것임.
    - 단점 : synchronized 기능 자체가 너무 느림.

  ```java
  public class Singleton {
      private static Singleton instance; // 최초 instance는 null
  
      private Singleton() {
      }
  
      public static synchronized Singleton getInstance() { // synchronized
          if (instance == null) {
              instance = new Singleton();
          }
          return instance;
      }
  }
  ```

  - 해결방법 2 : **이른 초기화 방식** 이용 **(Eager Initialization)**
    - JVM의 클래스 로더에 의헤 최초로 클래스가 로딩될 때부터 인스턴스를 메모리에 저장하는 방법.
    - 문제점 : 시작할 때부터 메모리를 차지하기 때문에 리소스가 낭비될 수 있음.

  ```java
  public class Singleton {
      public static Singleton instance = new Singleton(); // 클래스 로딩 시점에서 인스턴스 생성
  
      private Singleton() {
      }
  
      public static Singleton getInstance() {
          return instance;
      }
  }
  ```



- 그 외 많은 해결방식이 있다. [링크]( http://wiki.hash.kr/index.php/%EC%8B%B1%EA%B8%80%ED%86%A4%ED%8C%A8%ED%84%B4#.EC.9D.B4.EB.A5.B8_.EC.B4.88.EA.B8.B0.ED.99.94_.EB.B0.A9.EC.8B.9D)에 다른 방식들이 자세히 수록되어 있음.




