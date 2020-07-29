> Oracle Technical article의 Java 8: Lambdas, Part 1을 참고하여 쓴 포스트입니다.

## Overview

Java 8 부터 자바에 람다식이 도입되었습니다. 람다식은 익명 함수를 생성하기 위한 식입니다. 자바에서 람다식은 함수형 인터페이스를 구현하는 모습으로 사용되는데 이는 함수형 프로그래밍에 알맞습니다. 자바에서의 람다식은 함수형 인터페이스의 메소드를 간편하게 구현하여 사용하는데에 그 목적이 있습니다. 이제부터 람다식의 특징과 관련된 내용을 설명해보겠습니다!




## 람다식

#### 장단점

장점
- 메소드의 구현을 간결하게 하여 가독성을 높인다
- 람다로 구현하여 코드 줄 소비를 줄일 수 있다.
- 함수형 프로그래밍을 바탕으로 병렬 프로그래밍이 가능하다.
- [지연 연산](https://ko.wikipedia.org/wiki/%EB%8A%90%EA%B8%8B%ED%95%9C_%EA%B3%84%EC%82%B0%EB%B2%95)을 이용하여 향상된 퍼포먼스를 보여줄 수 있다.

단점
- 람다식이 남용된다면 오히려 프로그램의 코드를 이해하기 어려울 수 있다.
- 람다식을 재귀로 활용할 경우 까다롭다.

<br/>

#### 람다식의 기본 구성

```java
//ex1

//기존의 익명 내부 함수
Runnable r1 = new Runnable(){
  @Override
  public void run(){
    System.out.println("Hello world!!");
  }
};
r1.run();

//람다를 이용한 구현
Runnable r2 = () -> System.out.println("Hello world!!");
r2.run();
```

 위의 예시 ex1는 익명 내부 함수를 기존의 문법과 람다로 구현한 경우입니다. 같은 내용을 구현했을때 람다로 구현한 경우가 훨씬 간편하고 코드양을 줄일 수 있네요! 



```java
//ex2

// 1번 표현식 한개
Comparator<String> c1 = (String lhs, String rhs) -> lhs.compareTo(rhs);
int result1 = c1.compare("Hello", "World");

// 2번 표현식 두개
Comparator<String> c2 = (String lhs, String rhs) -> {
	System.out.println("Comparing "+ lhs + " and " + rhs);
	return lhs.compareTo(rhs);
};
int result2 = c2.compare("Hello","World"); 

//3번 타입 추론
Comparator<String> c3 = (lhs, rhs) -> lhs.compareTo(rhs);
int result3 = c3.compare("Hello", "World");


```

 람다식은 매개변수 집합 + 화살표 + 바디(코드 블록, 표현식) 형태로 구성되어 있습니다.

``` 
( 매개변수 ) -> { 표현식 };
```

 ex2에서 람다식 바디 부분이 하나의 표현식으로 되어있는 경우 1번 처럼 중괄호를 생략할 수 있고 return 키워드도 생략 가능합니다. 람다식의 바디의 표현식이 한개보다 많다면 2번 처럼 중괄호로 코드블록을 묶어서 사용하면 됩니다. 

 자바 8로 넘어오며 타입 추론 기능이 향상 되었습니다. 타입 추론이란 개발자가 타입을 따로 명시하지 않아도 컴파일러가 스스로 타입을 알아내는 경우를 말합니다. 현재 다른 언어들에서는 많이 사용되고 있는 특징입니다. ex2의 3번을 보시면 매개변수가 들어가는 부분인 (lhs,rhs) 에 타입이 명시되어 있지 않습니다. 컴파일러가 Comparator 함수형 인터페이스의 메소드를 보고 매개변수 타입을 스스로 판단하는 겁니다. 함수형 인터페이스는 바로 뒤에 다뤄보죠.



#### 함수형 인터페이스

함수형 인터페이스는 간단하게 말해서 메소드를 하나만 가지고 있는 인터페이스 입니다. 함수 구현만을 위해서 만들어진 인터페이스입니다. 함수형 인터페이스에는 Runnable, Comparator 등의 인터페이스가 있습니다. 사용자가 원하는 함수형 인터페이스를 직접 정의해서 사용할 수 있습니다. 함수형 인터페이스를 정의할 때 @FunctionalInterface 어노테이션을 이용해 정의합니다. 정의한 인터페이스 위에 어노테이션을 명시하면 해당 인터페이스가 함수형 인터페이스 임을 알리게 됩니다. 물론 메소드는 하나만 두어야 겠죠! 람다식은 이 함수형 인터페이스를 간편하게 구현하는 역할을 하는겁니다.

```java
@FunctionalInterface
interface Square{
    public int run(int number);
}

public void f(){
  int number = 10;
  Square s = (num) -> num*num; 
  int squaredNum = s.run(number);
}
```

함수형 인터페이스를 임의로 정의해주고 메소드에서 사용한 간단한 사례입니다.





## 마치며

- 람다식은 함수형 인터페이스를 간편히 구현하는 역할을 함
- 코드를 간결하게 만들어 줘서 코드 양을 줄여주고 가독성을 높여줌(남용되는 경우 제외)
- 람다식은 매개변수 집합 + 화살표 + 바디 로 구성 되어 있음.
- (매개변수) -> {표현식};
- 람다식 바디의 표현식이 한개일 경우(코드가 한줄일 경우) 중괄호 생략가능
- 매개변수에 타입을 따로 지정하지 않아도 됨
- 함수형 인터페이스는 메소드를 하나만 가지고 있는 인터페이스









<br/>

> Reference
>
> https://www.oracle.com/technical-resources/articles/java/architect-lambdas-part1.html

> https://www.daleseo.com/java8-no-more-loops/



