## 싱글턴 패턴(Singleton Pattern)

#### 싱글턴 패턴이란??

  클래스의 인스턴스를 단 한개만 생성하여 사용하는 디자인 패턴입니다. 싱글턴 패턴이 적용된 클래스에 대해 처음 호출 이후에 여러 차례 다른 생성자 호출이 있다하여도 처음 생성된 인스턴스만을 리턴합니다. 자바에서는 생성자를 private으로 선언하여 외부에서 생성자에 접근할 수 없도록 하고 해당 클래스의 getInstance() 라는 정적 메소드에 생성된 인스턴스를 받아쓰는 코드를 넣음으로서 싱글턴 객체를 사용할 수 있게 합니다. 하나의 인스턴스만 생성되기 때문에 객체 공유가 쉬워집니다. 싱글턴 패턴은 인스턴스가 오직 한개만 선언되어야 하는 경우에 많이 사용됩니다. 대표적인 예로 공통된 객체를 여러개 생성하는 DBCP(DataBase Connection Pool)이 있습니다. 레지스트리 , 로그 기록, 캐시 등등 에서 주로 사용됩니다. 스프링에서 Bean(스프링이 관리하는 객체)를 생성할 때에도 싱글턴으로 생성하는 경우가 많습니다. 사용자 요청이 많은 스프링에서 cost을 줄이기 위해 객체의 갯수를 한개만 생성하도록 제한하여 사용합니다. 싱글턴을 만들때 동시성을 고려하여 클래스를 설계해야 합니다. 그럼 지금부터 싱글턴을 만드는 여러 방법에 대해서 알아보죠.😎



## 사용하기

 싱글턴 패턴의 사용 예시에 대해 알아보죠! 

```java
public class Singleton{

    public void run(int i){
        Printer printer = Printer.getPrinter();
        printer.print("["+i+" 번째 객체] using "+printer.toString());
    }

    public static void main(String [] args){
        Singleton [] singletons = new Singleton [5];

        for(int i=0;i<singletons.length;i++){
            singletons[i] = new Singleton();
            singletons[i].run(i+1);
        }
    }
}

class Printer{

    private static Printer printer = null;
    private Printer() {}

    public static Printer getPrinter(){
        if(printer == null){
            printer = new Printer();
        }
        return printer;
    }

    public void print(String str){
        System.out.println(str);
    }
}
```

  Printer 클래스는 싱글턴 패턴으로 작성되어 있는 클래스 입니다. 클래스의 생성자를 private으로 선언합니다. 외부에서는 이 클래스를 객체로 생성할 수 없습니다. 클래스의 정적 필드로 printer라는 인스턴스를 작성해줍니다. 이 printer 인스턴스를 getPrinter() 정적 메소드에서 생성해주고 리턴해줍니다. 외부에서 getPrinter() 메소드를 호출하여 싱글턴 인스턴스를 사용하면 됩니다! 위와 같은 방법의 싱글턴 패턴을 **Lazy initialization** 이라고 합니다.

_***출력 결과***_

```
[1 번째 객체] using design_pattern.Printer@61bbe9ba
[2 번째 객체] using design_pattern.Printer@61bbe9ba
[3 번째 객체] using design_pattern.Printer@61bbe9ba
[4 번째 객체] using design_pattern.Printer@61bbe9ba
[5 번째 객체] using design_pattern.Printer@61bbe9ba
```

  코드를 실행해보면 위과 같은 출력 결과가 나옵니다. ***여러 객체에서 호출해도 처음 생성된 하나의 객체만 불린다***는 것을 알 수 있죠. 그러나 싱글턴 객체를 여러 멀티스레드에서 접근한다면 어떻게 될까요? 바로 다음에서 알아보죠!



## 문제점

```java
public class Singleton extends Thread{
    public Singleton(String name){
        super(name);
    }

    @Override
    public void run(){
        Printer printer = Printer.getPrinter();
        printer.print("[" + Thread.currentThread().getName()+"] using "+ printer.toString());
    }

    public static void main(String [] args){
        Singleton [] singletons = new Singleton [5];

        for(int i=0;i<singletons.length;i++){
            singletons[i] = new Singleton((i+1) +"-thread");
            singletons[i].start();
        }
    }

}

class Printer{

    private static Printer printer = null;
    private Printer() {}

    public static Printer getPrinter(){
        if(printer == null){
            printer = new Printer();
        }
        return printer;
    }

    public void print(String str){
        System.out.println(str);
    }
}
```

  위의 코드는 처음의 Printer 코드를 일부 수정한 코드입니다. 여러 쓰레드가 Printer 객체를 호출하는 모습을 볼 수 있습니다. 설명하기 전에 출력 결과부터 보고 가시죠.

_***출력 결과***_

```
[2-thread] using design_pattern.Printer@3cadc1b2
[3-thread] using design_pattern.Printer@45468adc
[1-thread] using design_pattern.Printer@73e3c327
[5-thread] using design_pattern.Printer@45468adc
[4-thread] using design_pattern.Printer@45468adc
```

  분명 싱글턴 패턴으로 작성된 객체에 접근하는데 하나의 객체가 아닌 여러개의 객체가 불리는 모습입니다. 싱글턴 패턴에 위배되는 결과네요. 이런 현상이 벌어지는 이유를 간단히 설명하겠습니다. getPrinter() 메소드를 살펴보면 `new Printer()`라는 생성 구문이 불리기 전에 printer 객체가 null 상태인지를 체크합니다. printer 객체가 null이라면 객체를 생성해주고 아니라면 생성해주지 않는 코드로서 객체를 단 한개만 생성할 수 있도록 하는 코드입니다. 그러나 여러 쓰레드에서 접근한다면 이런 코드 구조는 문제를 야기합니다. 문제가 되는 상황을 예를 들어보겠습니다. "Thread-1" 이 getPrinter()에 접근을 합니다. "Thread-1"은 printer객체가 null임을 확인하고 printer 객체를 생성할겁니다. 그리고 printer 객체를 리턴하겠죠. 그런데 만약에 "Thread-1"이 아직 getPrinter() 메소드를 마치지 않은 순간에 "Thread-2"가 getPrinter()에 접근한다면 "Thread-2"는 `if(printer == null)` 에서 printer가 null이라고 생각할 것이고 새로운 printer객체를 하나더 만들 것 입니다. 이런 상황은 **Thread-safe 하지 않은 상황**입니다. 이 문제를 해결하기 위한 몇 가지 방법을 소개해 보겠습니다.



## Thread-Safe한 싱글턴 패턴

  싱글턴 패턴을 멀티 쓰레드로 부터 안전하도록 하는 방법들입니다.

<br/>

#### 이른 초기화(Eager initialization)

```java
public class Singleton{

    private static Singleton instance = new Singleton();

    private Singleton(){ }

    public static Singleton getInstance() {
        return instance;
    }
  
}
```

  정적 멤버 변수를 선언함과 동시에 생성하는 방법입니다. 이른 초기화라는 이름에 걸맞게 클래스가 로딩되는 시점에서 생성됩니다. 가장 간단하게 작성이 가능한 방법입니다. 그러나 이 방법의 문제점은 **사용하지 않는 경우에도 인스턴스가 생성**이 되어 버리죠. 싱글턴 객체가 많은 자원을 사용하지 않으면 프로그램에 큰 부하가 없겠지만 많은 자원을 사용한다면 문제가 될 것 입니다.



#### synchronized 키워드를 이용한 싱글턴 (thread-safe singleton with synchronized)

```java
public class Singleton{

    private static Singleton instance = null;

    private Singleton(){ }

    public static synchronized Singleton getInstance() {
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }

}
```

  `synchronized` 키워드를 이용하여 thread-safe한 싱글턴 패턴을 만드는 방법입니다. `synchronized` 키워드를 사용하게 되면 getInstance() 메소드가 완전히 thread-safe한 동기화 블록이 됩니다. 그러나 이 방법에도 큰 문제가 존재합니다. `synchronized` 키워드는 기본적으로 프로그램의 성능을 매우 저하시킵니다. 심지어. 인스턴스가 존재하는 경우에도 매번 동기화 블록을 거쳐야 하므로 프로그램의 오버헤드가 엄청납니다. 위의 두 방법은 분명 멀티쓰레드로 부터 안전하지만 개선될 필요가 있어 보입니다. 



#### Double checked locking

```java
public static Singleton getInstance() {
        if(instance == null){
           synchronized (Singleton.class){
               if(instance == null){
                   instance = new Singleton();
               }
           }
        }
        return instance;
}
```

  위의 `synchronized` 키워드를 사용한 방식을 개선한 double checked locking 방식입니다. instance가 null인지 확인하고 null이면 `synchronized` 블록에 진입하도록 합니다. 이 방법은 instance가 생성이 되어 있는 상태에서 불필요하게 동기화 블록에 접근하지 않도록하여 보다 좋은 성능을 낼 수 있도록 합니다. 



#### Initialization on demand holder idiom

```java
public class Singleton{

    private Singleton(){ }

    private static class LazyHolder{
        public static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance(){
        return LazyHolder.INSTANCE;
    }

}
```

  이 방법은 조금 특이하게 내부 정적 클래스를 선언하여 그 내부 클래스에서 인스턴스를 생성하도록 하는 방식입니다. Singleton 클래스 자체에 인스턴스에 대한 어떠한 선언도 없기 때문에 getInstance()가 호출되기 전까지는 초기화되지 않습니다. LazyHolder안에 있는 변수 INSTANCE는 `final`키워드로 선언 되어 있습니다. `final`키워드로 작성되었기 때문에 변수에 재 할당이 되지않죠. `synchronized` 키워드 없이 동시성을 해결해주기 때문에 성능이 뛰어난 좋은 방법입니다. 

 .synchronized같은 키워드 없이 동시성을 해결해주기 때문에 성능이 뛰어난 좋은 방법입니다. 





## 마치며

- 싱글턴 패턴은 클래스의 인스턴스를 단 한개만 생성하여 사용하는 디자인 패턴
- DBCP, 레지스트리 , 로그 등에서 사용
- 동시성을 고려하여 설계해야함 (멀티쓰레드 환경)
- 사용법
  - [lazy initialization](#사용하기)
  - [eager initialization](#이른-초기화(eager-initialization))
  - [thread-safe singleton with synchronized](#synchronized-키워드를-이용한-싱글턴-(thread-safe-singleton-with-synchronized))
  - [double checked locking](#double-checked-locking)
  - [initialization on demand holder idiom](#initialization-on-demand-holder-idiom) (이게 제일 Good)





> Reference
>
> - [https://medium.com/webeveloper/%EC%8B%B1%EA%B8%80%ED%84%B4-%ED%8C%A8%ED%84%B4-singleton-pattern-db75ed29c36](https://medium.com/webeveloper/싱글턴-패턴-singleton-pattern-db75ed29c36)
> - https://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples#static-block-initialization









