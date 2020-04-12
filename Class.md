# 클래스
- 객체를 만들기 위한 설계도
- ```멤버```라고 통칭할 수 있는 메서드, 값, 변수, 타입, 객체, 트레잇, 클래스를 포함할 수 있음.

## 클래스 정의
- 가장 단순한 클래스 형태
```scala
class User

val user1 = new User
```
- 예약어 ```new```는 클래스의 인스턴스를 만들기 위해 사용
- ```User```클래스는 생성자를 정의하지 않았기 때문에 인자가 없는 기본 생성자를 갖는다.

생성자와 클래스 몸체를 정의하고자 한다면, 아래 참고
```scala
class Point(var x: Int, var y: Int) {
  
  def move(dx: Int, dy: Int): Unit = {
    x = x + dx
    y = y + dy
  }

  override def toString: String =
    s"($x, $y)"
}

val point1 = new Point(2, 3)
point1.x
println(point1)

--------------------------------
Output:

2
(2, 3)
```
이 ```Point``` 클래스에는 네 개의 멤버가 있음.  
- 변수 ```x```, ```y```
- 메서드 ```move```, ```toString```

다른 언어와 달리 기본 생성자는 클래스 서명부(signature)에 있다.
- ```(var x: Int, var y: Int)```

```move``` 메서드의 return 값 = ```()```  

```toString``` 메서드는 ```AnyRef```의 ```toString```을 오버라이드함


## 생성자
- 다음과 같은 기본 값을 제공하여 선택적 매개변수를 가질 수 있음
```scala
class Point(var x: Int = 0, var y: Int = 0)

val origin = new Point  // x and y are both set to 0
val point1 = new Point(1)
println(point1.x)  // 1
println(point1.y)  // 0

val point2 = new Point(y=2)
println(point2.y)  // 2
```
- 생성자는 인자를 왼쪽부터 읽으므로 ```y```값만 전달하고 싶다면 매개변수의 이름을 지정해야 한다. -> 명료성 향상을 위한 좋은 습관이다.


## Private 멤버와 Getter/Setter
- 멤버는 기본적으로 public  
- ```private``` 접근 지시자 사용 가능
```scala
class Point {
  private var _x = 0
  private var _y = 0
  private val bound = 100

  // Getter
  def x = _x
  // Setter
  def x_=(newValu: Int): Unit = {
    if (newValue < bound) _x = newValue else printWarning
  }

  // Getter
  def y = _y
  // Setter
  def y_=(newValue: Int): Unit = {
    if (newValue < bound) _y = newValue else printWarning
  }

  private def printWarning = println("WARNING: Out of bounds")
}

val point1 = new Point
point1.x = 99
point1.y = 101  // 경고 출력
```
- Getter: ```def x```, ```def y```
- Setter: ```def x_=```, ```def y_=```

기본 생성자에서 ```val``` or ```var```로 지정되지 않은 매개변수는 ```private``` 이다. -> class 내부에서만 참조 가능!
```scala
class Point(x: Int, y: Int)

val point = new Point(1, 2)
point.x  // 컴파일 x
```
