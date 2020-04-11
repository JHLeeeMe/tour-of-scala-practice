## 표현식
- 연산 가능한 명령문  
- 모든 표현식은 어떠한 값을 갖음.
```scala
1 + 1
println(1)
println(1 + 1)
println("Hello, " + "World!")
```

## 상수
- ```val``` 키워드  
- 재할당 x
```scala
val x = 1 + 1  // 타입 추론

val x: Int = 1 + 1  // 명시적 타입 지정
```

## 변수
- ```var``` 키워드
- 재할당 가능
```scala

var a = 123

a = 12345  // 재할당

var a: Int = 2828  // 마찬가지로 명시적으로 타입을 지정가능
```
 
## 블록
- ```{}```으로 표현식을 감싼 것  
- 블록 안 마지막 표현식의 결과가 블록 전체의 결과
```scala
println({
  val x = 1 + 1
  x + 1
})  // 3
```

## 함수
- 매개변수(parameter)를 가지는 표현식
- ```매개변수``` => ```표현식``` 형태
```scala
(x: Int) => x + 1  // 익명 함수

val addOne = (x: Int) => x + 1  // 이름 지정

val add = (x: Int, y: Int) => x + y  // 여러 매개변수

val getTheAnswer = () => 42  // 매개변수 x 
```

## 메서드
- ```def``` 키워드
```scala
def add(x: Int, y: Int): Int = x + y
println(add(1, 2))  // 3

// 두개의 매개변수 목록
def addThenMultiply(x: Int, y: Int)(multiplier: Int): Int = (x + y) * multiplier
println(addThenMultiply(1, 2)(3))  // 9
// same As 
// println(addThenMultiply(1, 2) {3})  <- 매개변수가 하나일 때 {} 사용가능

def name: String = System.getProperty("user.name")  // 매개변수 x
println("Hello, " + name + "!")
```

## 클래스
- ```class``` 키워드
- 생성자 매개변수를 갖는다.
- 모든 스칼라 표현식은 값을 가져야 함. -> ```()```로 쓰여진 ```Unit``` 타입의 싱글톤 값이 쓰인다. (java의 void와 유사)
```scala
class Greeter(prefix: String, suffix: String) {
  def greet(name: String): Unit = 
    println(prefix + name + suffix)
}

val greeter = new Greeter(Hello, ", "!")
greeter.greet("Scala developer")  // Hello, Scala developer!
```

## 케이스 클래스
- ```case class``` 키워드
- ```new``` 키워드 없이 인스턴스화 가능
```scala
case class Point(x: Int, y: Int)

val point = Point(1, 2)
val anotherPoint = Point(1, 2)
val yetAnotherPoint = Point(2, 2)
```
- 그리고 값으로 비교
```scala
if (point == anotherPoint) {
  println(point + " and " + anotherPoint + " are the same.")
} else {
  println("nope")
}  // Point(1, 2) and Point(1, 2) are the same.
```

## 객체
- ```object``` 키워드
- 객체 안의 메서드는 static 메서드 (?)
```scala
object IdFactory {
  private var counter = 0
  def create(): Int = {
    counter += 1
    counter
  }
}
```
- 객체 이름을 참조하여 접근
```scala
val newId: Int = IdFactory.create()
println(newId)  // 1
val newerId: Int = IdFactory.create()
println(newerId)  // 2
```

## 트레이트
- ```trait``` 키워드
- java의 인터페이스 느낌, python의 Mixin 느낌 -> 다중상속 가능
```scala
trait Greeter {
  def greet(name: string): Unit
}
```
- 기본 구현도 가질 수 있음.
```scala
trait Greeter {
  def greet(name: String): Unit = 
    println("Hello, " + name + "!")
}
```
- ```extends``` 키워드로 트레이트를 상속받을 수 있고, ```override``` 키워드로 구현 오버라이딩 가능
```scala
class DefaultGreeter extends Greeter

class CustomizableGreeter(prefix: String, postfix: String) extends Greeter {
  override def greet(name: String): Unit = 
    println(prefix + name + postfix)
}

val greeter = new DefaultGreeter()
greeter.greet("Scala developer")  // Hello, Scala developer!

val customGreeter = new CustomizableGreeter("How are you, ", "?")
customGreeter.greet("Scala developer")  // How are you, Scala developer?
```

## 메인 메서드
- 프로그램의 진입 지점
- JVM에선 ```main``` 이라는 메서드가 필요, 문자열 배열 하나를 인자로 가짐
```scala
object Main {
  def main(args: Array[String]): Unit =
    println("Hello, Scala developer!")
}
```
