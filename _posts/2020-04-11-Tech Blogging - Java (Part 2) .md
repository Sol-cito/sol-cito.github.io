---
layout: post
title: "JAVA 파헤치기 (Part 2)"
subtitle: "public static void main(String arg[]) 의 정체"
background: '/img/bg_technology.jpg'
categories: technology
---



### 코린이 시절 부트캠프에서부터 궁금했던 것



2020년 현재 내가 다니고 있는 회사는 입사 전 2주간의 부트캠프 과정이 있었는데, 그때 나는 난생 처음으로 IDE를 설치하고, Hello World를 콘솔에 찍었고, JAVA문법을 배웠다.

그런데 매번 Java로 System.out.print 를 할 때마다 정말 궁금했던 것이 있었다.

그것은 바로,

**"왜 모든 시작은 main 에서 시작되어야 하고, 왜 main method는 static이어야 하며, 왜 파라미터로 String[] arg 라는 것이 들어가는가?"** 하는 의문이었다.

JAVA를 만져본 사람들은 한번쯤 궁금해볼 내용이지만, 정작 내 주위에 이게 무슨 의미를 갖는지 물어봤을 때 막힘없이 술술 설명해줄 사람은 많이 없는 것 같다.

그래서 내가 알아보고 답을 찾아내기로 했다.

이 포스팅 내용은 내가 현재 진행중인 'The Elements of Computing Systems' 프로젝트의 챕터 6장 실습 내용과도 연결되는 부분이라,  운좋게도 이 포스팅을 통해 실습을 해결할 수 있었다.



---

### 메모장으로 JAVA 소스를 만들어보자!

우선 본인의 컴퓨터에 JDK 및 JRE가 설치되어있어야 한다.

(JDK와 JRE가 어떤 차이가 있는지도 궁금한데, 이건 다음 포스팅에서 알아보도록 하자)

![image-20200411020616543](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411020616543.png)



어떤 종류의 텍스트 에디터라도 상관없이, 우선 이렇게 우리 눈에 익숙한 java 소스를 직접 입력한다.

그리고 저장하는데, 이름을 저기 입력한 클래스명(Sol)과 동일하게 만들고 확장자를 .java로 하여 저장한다.

(사실 Sol은 public 클래스가 아니기 때문에 java파일명을 Sol로 안해도 되지만...헷갈리지 않기 위해서 동일하게 설정한다. public으로 선언되면 반드시 파일명이 같아야 한다.)

![image-20200411011948888](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411011948888.png)

그럼 이런 파일이 생길 것이고, 우리는 이 파일을 명령 프롬프트(cmd)로 실행시켜볼 것이다.

나의 경우 저 파일의 폴더 위치는 C가 아니라 P에 있으므로, 우선 P로 이동한다.

![image-20200411012121371](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411012121371.png)



다음, 이 JAVA파일을 complie해서 바이트코드로 만들어야 한다.

[지난 포스팅](https://soulfulsol.github.io/technology/2020/03/05/Tech-Blogging-Java-(Part-1)) 에서  살펴봤듯이, JAVA파일은 Java Compiler에 의해 바이트코드인 class파일로 변환되고, 이 class파일은 JVM에 의해 해당 OS에 맞는 기계어로 변환된다고 했다.

따라서 우리는 javac 명령어를 통해 해당 JAVA파일을 class파일로 바꾼다.

![image-20200411013035928](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411013035928.png)



자, 폴더에 Sol.class 파일이 생겨났다. 

그럼 이제 저 class파일을 java 명령어로 실행하면 우리가 입력해놓았던 System.print.out 명령어가 동작해야 한다.

![image-20200411013145724](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411013145724.png)



**Hello World!** 가 잘 출력되는 것을 확인할 수 있다!



**<참고>** 혹시 아래와 같은 메시지가 나온다면,

```
A JNI error has occurred, please check your installation and try again (...)
```

[Stackoverflow](https://stackoverflow.com/questions/22381202/a-jni-error-has-occurred-please-check-your-installation-and-try-again-in-eclips)의 answer를 참고하자. 답변을 요약하면 JDK 12 이상을 다운받아 설치하면 JRE도 함께 동일 버전으로 설치되어, 기존 JRE가 다른 버전이 이미 설치되어 있다면 컴퓨터가 confused한다는 답변이다. 즉 기존 JRE를 삭제하면 JDK 12를 설치할 때 함께 설치된 JRE 12만 남아있어 정상적으로 동작한다는 것.



---

### 그래서 public static void main(String arg[]) 의 정체가 뭔데?

왜 갑자기 메모장으로 뜬금없는 짓을 했을까? 사실 저 메모장 삽질로 public static void main(String arg[]) 의 정체를 설명할 수 있다.

지금부터 작성할 내용은 

https://www.youtube.com/watch?v=On9mzziI2pU

위 유투브 영상을 참고하였다. 인도풍의 억양을 가진 인자한 아저씨가 차분히 설명해주는데 자막은 없다..그래도 천천히 설명하니 다 알아들을 수 있다.

![image-20200411014057792](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411014057792.png)

위 화면을 보면서 의문을 하나하나 파헤쳐보자.



**1) 왜 main method는 class 안에 위치하는가? **

main은 프로그램이 시작하는 method이다. C++같은 프로그램에서는 main method가 클래스 안에 있지 않다.

그러나 JAVA는 OOP이고, OOP인 것을 보여주고자 main이 클래스 안에 위치한다. 

따라서, main method는 **어떤 class라도 그 안에 위치해서 프로그램을 executing하는 역할**을 한다. 단지 main이 위치한 class가 해당 프로그램의 starting point가 되는 것이다.

그럼 왜 그럴까?

![image-20200411013145724](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411013145724.png)



우리가 아까 위에서 실행시킨 위 Java 명령어는 JVM으로 들어가,  **Sol.main** 이 된다.

즉, P:\>JAVA Sol 은 사실은 **P:\>JAVA Sol.main** 이라는 명령어 - 해당 클래스 안의 main이라는 method를 실행시켜라 - 라는 명령어였던 것이다!

main을 JVM이 최초로 실행시키도록 설정되어있기 때문에, main이 해당 프로그램의 starting point로서 기능하게 된다.



**2) 왜 public static 인가 ? **

static으로 선언된 method는 object를 생성하지 않아도 calling할 수 있다.

따라서 JVM이 main method를 최초로 calling할 때, static으로 되어있기 때문에 객체를 생성하지 않고도 main을 실행시킬 수 있는 것이다.

이는 또한 외부에서 해당 method가 불려야 함을 의미하므로, private이 아닌 public 으로 설정되어야 함을 뜻한다.



**3) String[] arg 는 왜 파라미터로 들어가는가 ?**

우리가 위에서 cmd창에 입력한 명령(java, javac, dir 등..)은 전부 '**커맨드라인 명령어**'라고 불리는 것들이고, 저 cmd라는 놈은 '셸'또는 '터미널'이라 불리는 하나의 작은 프로그램이다.

이 터미널(혹은 셸)은 사용자의 명령어를 해석하여 그 응답을 터미널에 출력해준다.



![image-20200411013145724](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411013145724.png)



위에서 우리가 **P:\>java Sol** 이라고 입력한 것도 java라는 커맨드를 입력한 것이다.

만약 메모장(Notepad)을 열고싶다면, 아래와 같이 입력하면 된다.

![image-20200411021721129](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411021721129.png)



커맨드라인에 의해 메모장이 저절로 켜진다.

그럼 여기서, 만약 **P:\>Notepad newNote** 라고 입력한다면?

![image-20200411021827591](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411021827591.png)



보다시피 new.txt라는 파일을 열려고 시도했으나 찾을수가 없어서, 새로 만들겠냐고 묻는 창이 뜬다!

Notepad 뒤 new라는 명령어를 하나의 parameter로 받아서, **"parameter로 들어온 값의 이름을 가진 메모장 파일을 열어라"** 라는 method가 실행되고 있는 것이다.

Java도 이와 마찬가지로, Java 명령어 뒤에 커맨드라인 명령어를 붙일 수 있고, 이것이 바로 

public static void main(**String[] arg**) 부분에 들어가는 파라미터이다. 커맨드라인의 공백을 구분기호로 하여 String array가 만들어지고, 그것이 파라미터로 특정 기능을 하게 만들 수 있다 (파일경로를 지정한다거나).

그럼 테스트를 한번 해보자.

아래와 같이 메모장에 간단한 반복문 코드를 짜고 .java로 저장했다.

![image-20200411022301017](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411022301017.png)



그리고 마찬가지로 cmd를 통해 class파일을 만들고 "**I AM THE BEST**" 라는 커맨드라인을 실행시키면...

![image-20200411022527844](C:\Users\datae\AppData\Roaming\Typora\typora-user-images\image-20200411022527844.png)



이렇게 공백으로 구분지어진 String 배열이 차례대로 출력되는 것을 확인할 수 있다!

신기하지 않은가?

이제 당신도 메모장으로 충분히 코딩테스트 문제를 풀고 cmd로 콘솔을 찍을 수 있다....