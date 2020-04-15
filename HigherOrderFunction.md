# 고차 함수
- 다른 함수를 파라미터로 받거나, 수행의 결과가 함수
- scala에서 함수는 일급시민이라 가능

## 함수를 파라미터로 받는 고차함수
### 첫번째 예제
- 함수 ```f```와 값 ```v```를 받아서 ```f(v)```을 return 하는 고차함수 ```apply```를 정의해보자.
```scala
#!/usr/bin/env scala


def apply(f: Int => String, v: Int) = f(v)
```
- ```apply```메서드 활용
```scala
#!/usr/bin/env scala


class Decorator(left: String, right: String) {
  def layout[A](x: A) = left + x.toString() + right
}

object FunTest extends Add {
  def apply(f: Int => String, v: Int) = f(v)

  val deco = new Decorator("[", "]")
  println(apply(deco.layout, 7))
}

--------------------------------------------------
Output:

[7]
```

### 두번째 예제
- 고차 함수의 대표적 예 ```map```함수를 살펴보자.
```scala
#!/usr/bin/env scala


val salaries = Seq(200, 700, 400)
val doubleSalary = (x: Int) => x * 2
val newSalaries = salaries.map(doubleSalary)  // List(400, 1400, 800)
```
```doubleSalary```는 ```x```를 취하고 ```x * 2```를 리턴해주는 함수.  
```=>```의 왼쪽 튜플은 파라미터 리스트들 이고, 오른쪽은 return 값이다.  
3번째 라인에서 ```doubleSalary```함수는 salaries의 요소들을 인수로 취한다.  

위 코드를 줄이기 위해 ```익명 함수```를 써보자.
```scala
#!/usr/bin/env scala


val salaries = Seq(200, 700, 400)
val newSalaries = salaries.map(x => x * 2)  // List(400, 1400, 800)
```
```x```의 타입은 선언되지 않았다. 그 이유는 컴파일러가 map함수의 예상되는 타입을 통해 타입추론이 가능하기 때문이다.  

나중에 살펴볼 Currying을 통해 훨씬 더 간단하게도 표현 가능하다. 
```scala
#!/usr/bin/env scala


val salaries = Seq(200, 700, 400)
val newSalaries = salaries.map(_ * 2)
```
컴파일러는 이미 파라미터의 타입(a single Int)을 알고있기 때문에,  
우리는 ```=>```의 오른쪽만 표현하면 된다.(```_``` is ```x``` in the previous example)

## 메서드를 고차 함수의 인수로
- 스칼라 컴파일러는 메서드를 함수로 강제한다. 그러므로
- 메서드를 고차함수의 인수로 넣을 수 있다.
- 위 ```apply``` 예제도 이러한 이유로 가능했던 것
```scala
#!/usr/bin/env scala


case class WeeklyWeatherForecast(temperatures: Seq[Double]) {
  
  private def convertCtoF(temp: Double) = temp * 1.8 + 32

  def forecastInFahrenheit: Seq[Double] = temperatures.map(convertCtoF)
}
```
고차 함수```map```의 인수로 ```convertCtoF```메서드를 취한다.  
이게 가능한 이유는 컴파일러가 ```convertCtoF```메서드를 ```x => convertCtoF(x)``` 함수로 강제하기 때문이다.  
즉, 그 전 예제들과 같이 ```map```함수의 인수로 익명함수 ```x => convertCtoF(x)```를 취했다고 볼 수 있다.(?)

## 고차 함수를 쓰는 이유
- 여러 이유중 하나는 반복되는 코드를 줄일 수 있다는 것

아래 코드는 고차 함수를 쓸 때와 쓰지 않았을 때를 비교함. 더 장황해졌지만..
```scala
#!/usr/bin/env scala


// 쓰지 않았을 때
object SalaryRaiser {

  def smallPromotion(salaries: List[Double]): List[Double] =
    salaries.map(salary => salary * 1.1)

  def greatPromotion(salaries: List[Double]): List[Double] =
    salaries.map(salary => salary * math.log(salary))

  def hugePromotion(salaries: List[Double]): List[Double] =
    salaries.map(salary => salary * salary)
}

// 쓸 때 (함수를 인수로 받는 고차함수)
object SalaryRasier {

  private def promotion(salaries: List[Double], promotionFunction: Double => Double): List[Double] =
  salaries.map(promotionFunction)

  def smallPromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, x => x * 1.1)

  def greatPromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, x => x * math.log(x))

  def hugePromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, x => x * x)
}
```

## 함수를 return하는 고차 함수
```scala
#!/usr/bin/env scala


def urlBuilder(ssl: Boolean, domainName: String): (String, String) => String = {
  val schema = if (ssl) "https://" else "http://"

  // return 되는 함수
  (endpoint: String, query: String) => s"$schema$domainName/$endpoint?$query"
}

val domainName = "www.example.com"
def getURL = urlBuilder(ssl=true, domainName)

val endpoint = "users"
val query = "id=1"
val url = getURL(endpoint, query)  // "https://www.example.com/users?id=1": String
```
위 코드에서  
urlBuilder의 return 타입은 ```(String, String) => String```이고,  
urlBuilder의 return 값은 ```(endpoint: String, query: String) => s"https://www.example.com/$endpoint?$query"```익명함수 이다.
