# 중첩 메서드
- 메서드 내부에 정의된 메서드
- C, Java 등에서는 지원을 안함

## 예제 1
- 중첩은 여러 겹으로 정의가능
```scala
#!/usr/bin/env scala


object GreetingBot {
  def main(args: Array[String]): Unit = {
    hi
  }

  // Method
  def hi = {
    println("Hi")
    hello

    // First Nested Method
    def hello = {
      println("Hello")
      howAreYou

      // Second Nested Method
      def howAreYou = {
        println("How are you?")
      }
    }
  }
}

----------------------------------------------
Output:

Hi
Hello
How are you?
```

## 예제 2
아래 코드는 정수 리스트에서 지정된 값보다 작은 값을 추출해주는 ```filter```함수를 담고있는 Filter 오브젝트이다.
```scala
#!/usr/bin/env scala


object Filter extends App {
  def filter(xs: List[Int], threshold: Int): List[Int] => List[Int] = {

    // Nested Method
    def process(ys: List[Int]): List[Int] = {
      if (ys.isEmpty) ys
      else if (ys.head < threshold) ys.head :: process(ys.tail)
      else process(ys.tail)
    }

    process(xs)
  }
  println(filter(List(1, 9, 2, 8, 3, 7, 4), 5))
}

---------------------------------------------------------------------
Output:

List(1, 2, 3, 4)
```

위 코드에서 보이듯이 ```process```함수는 외부에서 정의된 값 ```threshold```(filter 함수의 파라미터 값)를 참조한다.  
위 코드에서 ```process```함수는 재귀를 통해 유사 ```for```문을 표현한다  
```참고) head는 List의 첫번째 값, tail은 head를 제외한 나머지를 리턴한다.```


## Java의 경우
- Java는 중첩 메서드를 지원하지 않는다고 했다.
- Java에서의 메서드 안의 메서드를 정의 방법을 보자.

### 방법 1
- Interface를 이용한 형태
```java
public class Test1 {

  interface myInterface {
    void run();
  }

  static Foo() {
    myInterface r = () -> {
      System.out.println("interface!!!!!!!!");
    }
    r.run();
  }

  // main Method
  public static void main(String[] args) {
    Foo();
  }
}

-----------------------------------------------
Output:

interface!!!!!!!!
```

### 방법 2
- Local Class를 이용한 형태
```java
public class Test2 {

  static Foo() {
    // Local Class
    class Local {
      void fun() {
        System.out.println("local!!!!!!!");
      }
    }
    new Local().fun();
  }

  // main Method
  public static void main(String[] args) {
    Foo();
  }
}

----------------------------------------------
Output:

local!!!!!!!
```

