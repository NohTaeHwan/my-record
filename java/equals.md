# equals() 와 == 의 차이는?

### ==

- 연산자
- boolean을 리턴
- 두 객체 자체를 비교함

### equals()

- 메소드
- boolean을 리턴
- 두 객체의 값을 비교함

객체의 비교 , 객체의 값의 비교의 여부가 == 와 equals()의 가장 큰 차이. 

### 객체와 값의 차이는 무엇일까

<img width="312" alt="example2" src="https://user-images.githubusercontent.com/29722673/98330781-7e107100-203e-11eb-8080-7ac7151294c2.png">


왼쪽 빨간 박스 x,y,z는 String 값을 담는 변수, 파란색 박스는 객체, 그리고 객체 안에 들어있는 "hello"가 객체의 값이다. x가 가리키고 있는 (1)객체와 y,z가 가리키고 있는 (2)객체는 서로 다른 객체이다. 그렇기에 x == y 의 값은 false 가 될 것이다. x , y 가 가르키고 있는 객체는 다르지만 객체의 값을 살펴보면 "hello"로 동일하다. 두 변수가 가리키는 객체는 달라도 값은 같기 때문에 a.equals(b)의 값은 true가 된다. 각 객체 (1)과 (2)는 서로 다른 공간을 차지하고 있는 별도의 공간을 차지하고 있지만 값은 같다. 예를 들어 두 사람이 있는데 한명은 서울에 사는 '홍길동'이고 다른 한명은 부산에 사는 '홍길동'이라고 가정해보자. 두 명의 홍길동은 서로 이름이 같으나 다른 존재이다. 이 때 이름이 같은 것을 "객체의 값"이 같은 것과 비슷하다고 볼 수 있고 서로 다른 존재 라는 것을 "객체"가 다른것과 비슷하다고 보면 된다.



### 예시

```java
	String a = new String("hello");
        String b = new String("hello");
        String c = "hello";
        String d = "hello";
        String e = a;


        // ==
        System.out.println(a == b); // 1. false
        System.out.println(a == c); // 2. false
        System.out.println(c == d); // 3. true
        System.out.println(a == e); // 4. true

        //equals()
        System.out.println(a.equals(b)); // 5. true
        System.out.println(a.equals(c)); // 6. true
        System.out.println(c.equals(d)); // 7. true
        System.out.println(a.equals(e)); // 8. true
```







