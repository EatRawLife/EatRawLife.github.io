---
title: 자바 관련 정리(sorting)
date: 2024-08-14
permalink: /posts/2024/08/java01/
tags:
  - Java
---

Arrays 클래스  
배열과, 배열을 리스트 객체로 변경하는 내용을 가지고 있음.  
모든 메소드들은 참조된 배열이 null 이면 **NullPointerException**을 던짐.(별도 명시된 경우 제외) 


Stream
자바의 java.util.stream 패키지에 정의됨.  
흐르는 듯 연속으로 들어오는 데이터를 다루는데 사용되는 패키지.  
collection, Array,file등을 Stream으로 만들어 줄 수 있다. 

lambda  
인터페이스의 구현에서 사용됨

[Lambda Expressions in Java](https://www.youtube.com/watch?v=tj5sLSFjVj4)

예를 들어서 printable이란 인터페이스가 있다면 이를 구현한 클래스는 반드시 인터페이스속 print 메소드를 구현해야한다.  
그런데 인터페이스가 가진 메소드가 딱하나라면 사실 중요한것은 그 인터페이스 자체가 아닌 안에 있는 print 메소드 하나 일 것이다. 
즉 중요여부는 인터페이스를 구현했을 때 내부의 print메소드가 어케 구현 되는가 이다.
기존에는 어떤 클래스가 printable을 구현한다 표시하고(class ~~ implemets printable) 다음에 구현하려는 print 메소드를 내부에 구현한다. 그렇게 해야만 해당 객체는 Printable 변수로써 호출이 가능하기 때문에 비로소 printable 파라미터로 호출되는 메소드등을 실행할 수 있게 된다 . 

여기서 람다의 역할이 나오게 되는데, 람다를 사용하면, 위의 상속하는 객체 생성 없이도 printable 파라미터를 쓰는 메소드를 사용가능하다. 어떻게? 메소드의 파라미터로 우리가 구현하고자하는 (printable 메소드의 구현인) print 메소드를 인자로 넘겨주는 것이다.  
개념 변화 : 기능을 구현한 객체 넘겨줌 -> 구현된 기능만 넘겨줌.  
구현시 바뀌는 것 적어두기. 
기존: printable 구현 클래스 생성 > 메소드 구현 > 객체 생성 > printable 파라미터 사용가능  
람다: printable 파라미터로 print()의 구현 넘겨줌 . 끝.  
'구현'을 마치 변수러럼 넘겨준다!

인터페이스가 위처럼 하나의 추상메소드만 지닌 경우 functinal interface라고 한다.  
만약 인터페이스가 functinal interface면 위에 @FuntionalInterface라고 적어두면 다른 추상메소드를 내부에 추가하면 오류가 난다.(static 이나 일반 메소드 추가는 ok)
람다는 funtinal interface에 대해서만 동작하기 때문.  
만약 functinal interface가 아닌 환경에서 비슷한 작업을 하고 싶다면 무명 클래스를 이용해야한다. 

람다를 한마디로 요약하자면 람다는 funtinal interface의 가장빠른 구현 shortcut이라고 할 수 있다. 








