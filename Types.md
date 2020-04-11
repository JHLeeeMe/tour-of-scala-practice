Scala에선 모든 값이 하나의 type을 갖는다.  
![scala-types](https://user-images.githubusercontent.com/31606119/79055975-85c88c80-7c8c-11ea-8a48-068320ebe487.png)

# Scala Type Hierarchy
## ```Any```
- 모든 types의 supertype
- subClasses: ```AnyVal```, ```AnyRef```
- ```equals```, ```hashCode```, ```toString``` 메서드 등이 정의 돼있음

## ```AnyVal```
- value types (like primitive type in Java)
- non-nullable
- ```Unit```은 ```()```로만 선언된다. 의미있는 정보를 담고있지 않다. (like void in Java or C)
- 모든 함수들은 어떠한 값을 리턴하므로 return type으로 ```Unit```이 자주 쓸모있다. 

## ```AnyRef```
- reference types (like reference type in Java)
- value type이 아닌 모든 타입, User-Defined type 포함
- 자바 실행 환경에서 사용된다면 ```AnyRef```는 ```java.lang.Object```


아래는 다양한 타입의 값을 list에 담아보았다.
```scala
#!/usr/bin/env Scala


val list: List[Any] = List(
  "a string",
  732,  // an integer
  'c',  // a char
  true,  // a boolean value
  () => "스트링 값을 리턴하는 익명함수"
)

list.foreach(e => println(e))

----------------------------------
Output:

a string
732
c
true
<function>
```
```list```의 type은 ```List[Any]```이다.  
리스트 안의 엘러먼트들의 타입들은 모두 ```scala.Any```의 인스턴스이므로 에러가 안난다.


# Nothing and Null
## ```Nothing```
- 모든 types의 subtype.
- 값이 없음을 의미하는 type
- 예외 발생, 무한루프와 같은 비 종료 신호를 보내는 용도로 사용 (즉, 값으로 평가되지 않는 표현식의 타입 또는 정상적으로 반환되지 않는 메서드).

## ```Null```
- 모든 Ref types의 subtype
- 예약어 ```null```로 식별되는 단일 값을 갖음
- 주로 JVM 언어와의 상호 운용성을 위해 제공 (Scala 코드에서는 거의 사용 x)



Null is a subtype of all reference types (i.e. any subtype of AnyRef). It has a single value identified by the keyword literal null. Null is provided mostly for interoperability with other JVM languages and should almost never be used in Scala code. We’ll cover alternatives to null later in the tour.
