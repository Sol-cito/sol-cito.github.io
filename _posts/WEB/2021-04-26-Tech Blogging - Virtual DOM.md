---
layout: post
title: "[WEB] React는 왜 빠른가? - Virtual DOM"
subtitle: "Session and cookies are cached to store something"
background: '/img/bg_technology.jpg'
categories: technology-web
---


핫한 리액트를 써서 SPA 프로젝트를 하나 완성했고,

현재는 React를 Front-end로, Spring Boot를 Backend-end로 하여 웹 프로젝트를 하나 진행하고 있다.

그래서 이번 기회에 왜 React가 빠른지, **Virtual DOM**은 무엇인지에 대해 간략히 정리해보고자 한다.

---

#### BOM and DOM

모든 웹 서비스는 **브라우저** 위에서 제공된다.

브라우저는 본인의 다양한 기능을 제공(API)하기 위해 **객체 모델**을 사용하는데,  이 모델을 **BOM(Brouser Object Model)**이라 한다.

Javascript를 배울 때 window라는 객체에 대해 배울것이다.

window 객체는 브라우저의 요소들, 엔진, 모든 변수를 담은 **최상위 객체(Java에서는 Object클래스 정도..?)**이다.

window처럼 **웹브라우저 윈도우 및 웹페이지의 일부를 제어**할 수 있는 객체들을 **BOM**이라 하며, 

BOM에는 **screen, location, history, navigator**등이 존재한다.

<br/>

window 객체는 **document**객체를 포함한다.

document객체는 웹 페이지 그 자체를 의미하며, **DOM**의 최상위 객체이다.

Dom이란, **document를 이루고 있는 각각의 node들을 tree구조로 표현하여 XML이나 HTML을 다루는 인터페이스**이다.

즉, **HTML 요소(element)**를 **객체(object)**로 취급하여 **Tree형태**로 표현한 하나의 **구조 체계**라 할 수 있다.

![image](https://www.w3schools.com/whatis/img_htmltree.gif)

(출처 : [https://www.w3schools.com/whatis/whatis_htmldom.asp](https://www.w3schools.com/whatis/whatis_htmldom.asp))

위 그림과 같이 최상단 Document 객체에서 시작하여 각각의 element들이 Tree형태로 뻗어나가고 있다.

이러한 구조를 **DOM**이라 하며, 

DOM 형태의 HTML에 있는 **Tag** 들로 웹페이지의 뼈대를 만드는 작업을 **모델링**이라 하며,

각 Tag의 속성들을 이용하여 뼈대에 살을 붙이는 작업을 **렌더링(Rendering)**이라 한다.

(SPA는 기본적으로 클라이언트 사이드 렌더링이다. 페이지가 바뀔 때 마다 서버로 요청을 하는 것이 아님.)

**리액트가 빠르고 핫한 이유도 이 DOM과 Rendering을 어떻게 다루느냐와 관련이 있다.**

---

#### Brouser DOM 과 Virtual DOM

브라우저에 어떤 변화가 생긴다고 가정하자.

만약 DOM이 하나라면, Tree구조의 특징 상 하나의 node가 변하면 그에 딸린 부모, 자식 node에도 변화가 생기므로,

**Brouser DOM은 변화가 생길 때 마다 각 node와 subnode를 그때그때 rendering해야만 한다.**

즉 View의 변화로 20개의 노드를 수정해야 한다면,

rendering 작업도 20번이 추가로 발생하게 되는 점이다.

이 또한 컴퓨터 연산 작업이므로 빈번한 rendering이 발생할 시 성능이 저하될 우려가 있다.

거꾸로 말하면, DOM을 '최소한'으로 조작하면서 변화를 반영하는 것이 성능을 향상하는 방법이라 할 수 있다.

<br/>

리액트는 **Virtual DOM**으로 **변화된 부분만 한꺼번에 Brouser DOM에 적용하는 방식**으로 성능 향상을 꾀한다.

이를 리액트 공식 홈페이지에서는 **재조정(Recolciliation)**이라고 하며,

자세한 내용은 [여기](https://ko.reactjs.org/docs/faq-internals.html)와 [여기](https://ko.reactjs.org/docs/reconciliation.html)에서 확인가능하다(리액트 공식문서).

Virtual DOM은 실제 DOM보다 가벼운 copy DOM 이라고 할 수 있다.

UI에 변화가 생기면 우선 이 Virtual DOM에 전체 UI를 리랜더링 한 후, 기존 Virtual DOM과 변화된 새 Virtual DOM을 **비교(diff)**한다.

바뀐 부분이 전부 확인되면, **이 부분을 한꺼번에 실제 DOM에 반영하여 성능을 높인다**.

Virtual DOM은 **렌더링과정이 없기 때문에 실제 DOM보다 연산비용이 적다.**

즉, View의 변화로 20개의 노드에 영향이 있다면,

React는 **20개의 노드 변화를 diff 알고리즘으로 캐치하고 필요한 DOM트리만 한 번 Update하는 것으로 성능을 높인다.**

<br/>

결론적으로,

기존의 방식은 DOM element 가 바뀔 때 마다 **그에 맞춰 Rendering 작업이 여러번 발생하는데,**

React는 **변경된 elements들이 무엇인지 Virtual DOM에서 확인한 후 한꺼번에 DOM에 반영시켜 Rendering 횟수를 줄인다**.

