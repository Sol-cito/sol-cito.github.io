---
layout: post
title: "[OOP] OOP 바로알기 (3)"
subtitle: "상속x, 조립o"
background: '/img/bg_technology.jpg'
categories: technology-oop
---

### 상속보단 조립

상속의 문제점은 무엇일까?

**상위 클래스와 하위 클래스의 연결이 강하므로,**

**상위 클래스의 변경이 하위 클래스에 영향을 미치므로 변경이 어렵다**

또한, **클래스의 개수가 점점 많아진다**는 단점 또한 존자한다.

새로운 기능의 추가로 하위 클래스를 만들 때, 다수의 상위 클래스에서 상속을 받을 수 있기 때문이다.

또다른 문제점으로, **상속의 오용**을 들 수 있다. 아래 코드를 보자.

```java
public class CustomContainer extends ArrayList<Stuff>{
    private int currentSize;
    private int maxSize;
    
    public CustomContainer(int maxSize){
        this.maxSize = maxSize;
    }
    
    public void putStuff(Stuff stuff) throws NotEnoughSpaceException {
        if(isFull) throw new NotEnoughSpaceException();
        super.add(stuff); // 상속받은 ArrayList의 add메소드 사용
        currentSize++;
    }
    
    public boolean isFull(){
        return currentSize >= maxSize;
    }  
}
```

ArrayList를 상속한 자식 ArrayList인 CustomContainer이다.

겉으로 보기엔 별 문제가 없어 보이지만...

실제 개발할 때, 저 클래스는 ArrayList의 add를 활용하여 putStuff 메소드를 정의하고 있는데,

**개발자 입장에서는 putStuff 뿐 아니라 부모 클래스인 add() 메소드 또한 사용할 수 있다.**

부모클래스의 add()를 제어할 수 없기 때문이다.

따라서, putStuff() 메소드가 의도한 'full 이 아닐 때만 stuff를 add하라'는 규칙이 적용되지 않는,

**부모클래스의 add()메소드를 곧바로 활용하는 실수**가 나올 수 있다.

이런 상황에서는 maxSize를 넘어서는 stuff를 add하는 결과를 초래하고,

결국 추후 비즈니스 로직 진행에 큰 문제가 발생할 수밖에 없다.

이러한 경우, **상속보다는 '조립'을 권장하게 된다.**

<br/>

#### 조립

조립이란 별거 아니고, 특정 클래스에 다른 클래스의 기능이 필요하다면,

그 클래스의 **객체**를 **필드**에 넣어주는 것을 의미한다.

위에서 예로 들었던 CustomContainer 코드를 조립의 형태로 바꾸면,

```java
public class CustomContainer {
    private int currentSize;
    private int maxSize;
    private List<Stuff> list = new ArrayList<>(); // list필드 추가
    
    public CustomContainer(int maxSize){
        this.maxSize = maxSize;
    }
    
    public void putStuff(Stuff stuff) throws NotEnoughSpaceException {
        if(isFull) throw new NotEnoughSpaceException();
        list.add(stuff); // list필드의 add메소드 사용
        currentSize++;
    }
    
    public boolean isFull(){
        return currentSize >= maxSize;
    }  
}
```

위와 같이 변경한다면,

**이제는 CustomContainer 클래스가 더이상 상속받은 add()메소드를 제공하지 않게 된다**.

따라서, 개발자는 오직 putStuff 메소드만 사용하게 될 것이다.

<br/>

위와 같은 상속의 문제점은 사실, 

**CustomContainer 클래스는 사실 List 클래스를 상속하지 말았어야 했기 때문**에 발생하였다.

Container 클래스는 말 그대로 짐(stuff)를 managing하는 클래스이며,

그 기능 중 '일부'로 List가 필요한 것이지,

**Container 클래스가 그 자체로 List가 되어서는 안되기 때문이다.**

따라서 개발자는 상속 기능을 사용할 때는,

**해당 클래스가 정말로 부모를 상속한 것이 맞는지**에 대한 면밀한 고민을 해야 한다.

\---

위 내용은 '인프런' 최범균 강사님의 객체지향 입문 강의 내용을 요약 정리한 것임을 밝힙니다.

(https://www.inflearn.com/)



