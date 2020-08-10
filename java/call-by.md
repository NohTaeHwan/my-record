# 함수 호출(call by reference , call by value)

## Call by reference , Call by value에 대하여

위의 두 용어는 함수를 호출하는 경우에 인자 값을 어떻게 받아오는지에 대한 방식이다. 값으로 인자를 넘겨받거나 참조를 인자로 넘겨받는데에서 두 방식은 차이를 보인다. 두 차이를 설명하기에 앞서 변수의 주소에 대해서 설명할 필요가 있다. 변수는 각각의 공간을 가지고 있는데 그 공간은 모두 주소를 가지고 있다. 사람들이 사는 집에 주소가 있는 것과 같은 개념이다. 밑에 예시 그림을 보면 word라는 변수는 "abc"라는 값을 담는 공간과 주소를 담는 공간을 가지고 있다. 



<img src="/Users/taehwan/Library/Application Support/typora-user-images/image-20200810153527295.png" alt="image-20200810153527295" style="zoom:40%;" />



- Call by value : 값을 인자로 넘겨받는 방식이다.  값을 넘겨받을 때 해당 값을 복사하여 그 복사된 값을 넘겨 받는 것이다. 위의 그림의 word를 넘겨받는다고 가정하면 word라는 변수 자체가 아니라 "abc"라는 값의 복사본을 넘겨받는다고 할 수 있다. Call by value는 값의 복사본이 이동하는 개념이기 때문에 기존의 변수값에는 아무런 변화가 없다. 기존 값이 변화로부터 안전하다는 장점이 있다. 그러나 복사본이 생기기 때문에 메모리를 더 사용해야 한다.
- Call by reference : 참조를 인자로 넘겨받는 방식이다. 다시 말해 해당 변수의 주소값을 넘겨받는 것이다. 위의 그림의 word를 넘겨받는다고 가정하면 word 변수의 주소값 1000을 넘겨받는 것이다. Call by reference는 값의 주소값을 넘겨주기 때문에 기존의 변수값이 바뀔 수가 있다. 복사의 과정이 없기 때문에 간결하고 빠르게 처리가 진행된다. 그러나 기존 값이 영향을 받기 때문에 리스크가 있다. 



## 예시

함수 호출 방식을 설명할때 대표적으로 등장하는 예시가 있다. 바로 swap()함수. 두 변수를 swap() 함수에 넘겨 주었을때 서로 값이 뒤바뀌었는지 확인하는 함수이다.

##### call by value

```c
#include <stdio.h>

void swap(int a , int b){
    int tmp = a; 
    a = b;
    b = tmp;
}

int main(){

    int a = 10 , b = 20;

    printf("swap() 함수 호출 전 : %d %d\n",a,b);
    swap(a,b);
    printf("swap() 함수 호출 후 : %d %d\n",a,b);
}
```

출력 결과

```
swap() 함수 호출 전 : 10 20
swap() 함수 호출 후 : 10 20
```

a와 b의 값을 swap() 함수에 넘겨준 예시이다. 예시에서 a와 b의 값은 각각 10 , 20이다. 위의 예시 처럼 값을 넘겨주면 a 와 b 변수 자체가 넘어가게 되는 것이 아니라 10과 20이라는 값의 복사본이 넘어가는 것이다. swap() 함수에서 각 변수의 값을 바꾸는 로직이 수행되더라도 main() 함수에 있는 a와 b에는 아무런 영향도 끼치지 못한다. swap() 함수에서 값 변환 로직이 수행된 a와 b는 그저 main()함수에서 넘겨준 a와 b의 복사본이기 때문이다.



##### call by reference

```c
#include <stdio.h>

void swap(int *a , int *b){
    int tmp = *a; 
    *a = *b;
    *b = tmp;
}

int main(){

    int a = 10 , b = 20;

    printf("swap() 함수 호출 전 : %d %d\n",a,b);

    swap(&a,&b);

    printf("swap() 함수 호출 후 : %d %d\n",a,b);
}

```

출력 결과

```
swap() 함수 호출 전 : 10 20
swap() 함수 호출 후 : 20 10
```

출력 결과를 보면 call by value와는 다르게 a와 b값이 바뀌어 있는 결과를 볼 수 있다. swap() 함수를 call by value와는 다르게 a와 b의 포인터를 인자로 받는 방식으로 바꿔주었다. main함수에서 호출하는 swap(a,b)도 swap(&a,&b)로 수정하여 a와 b의 주소값을 넘겨주었다. swap()함수가 수행되며 a와 b의 주소값 자체가 바뀌기 때문에 main()함수에 있는 a와 b도 영향을 받게 된다. 





## 마치며

추가적으로 Call by assignment라는 개념이 있다. 이는 파이썬 언어에서 나오는 개념이다. 추후에 따로 다루는걸로.
