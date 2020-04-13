# Tuple
- 정해진 요소를 가지는 값, 각 요소는 고유한 타입을 가짐.
- immutable
- 특히 메서드로부터 여러개의 값을 return하는데 편리

## 정의
- 아래는 두개의 엘러먼트를 갖는 튜플
```scala
#!/usr/bin/env scala


val ingredient = ("Sugar", 25)

```
위 코드는 ```String```, ```Int``` 타입의 엘러먼트를 포함한다.  
상수 ```ingredient```의 추론된 타입은 ```Tuple2[String, Int]```의 약칭인 ```(String, Int)``` 이다.  
Scala에서는 튜플들을 나타내기 위해 다음의 클래스들을 사용한다.  
```Tuple2```, ```Tuple3```, ..., ```Tuple22```. 각 클래스는 파라미터 타입의 갯수 만큼의 엘러먼트들을 가지고 있다.

- Tuple2를 생성하는 특별한 방법
```scala
#!/usr/bin/env scala

val ingredient2 = "Sugar" -> 25  // (Sugar, 25)

// ```Tuple2[Int, String]```, ```Tuple2[String, Int]```, ```Tuple2[Char, String]```타입의 엘러먼트를 가지고있는 Tuple3
// Tupel3[Tuple2[Int, String], Tuple2[String, Int], Tuple2[Char, String]]
// ((1,str),(aa,4),(c,soso))
val test = (1 -> "str", "aa" -> 4, 'c' -> "soso")  
```
위 코드에서의 표현 ```->```으로 튜플2를 생성했는데 이 표현은 Map을 정의할때도 볼 수 있다.
```scala
#!/usr/bin/env scala


val mapTest0 = Map((1, "first"), (2, "second"))
val mapTest1 = Map(1 -> "first", 2 -> "second")

mapTest0(2)  // second
mapTest1(2)  // second
```
한마디로다가 Map에 Tuple2 타입의 값을 넣으면  
튜플의 0번째 인덱스가 key로 1번째 인덱스가 value로 간다.

## Tuple 클래스 살펴보기
- 위에서 만든 Tuple2 클래스만 살펴보자.
```scala
#!/usr/bin/env scala


/*
 * Scala (https://www.scala-lang.org)
 *
 * Copyright EPFL and Lightbend, Inc.
 *
 * Licensed under Apache License 2.0
 * (http://www.apache.org/licenses/LICENSE-2.0).
 *
 * See the NOTICE file distributed with this work for
 * additional information regarding copyright ownership.
 */

// GENERATED CODE: DO NOT EDIT. See scala.Function0 for timestamp.

package scala


/** A tuple of 2 elements; the canonical representation of a [[scala.Product2]].
 *
 *  @constructor  Create a new tuple with 2 elements. Note that it is more idiomatic to create a Tuple2 via `(t1, t2)`
 *  @param  _1   Element 1 of this Tuple2
 *  @param  _2   Element 2 of this Tuple2
 */
final case class Tuple2[@specialized(Int, Long, Double, Char, Boolean/*, AnyRef*/) +T1, @specialized(Int, Long, Double, Char, Boolean/*, AnyRef*/) +T2](_1: T1, _2: T2)
  extends Product2[T1, T2]
{
  override def toString(): String = "(" + _1 + "," + _2 + ")"
  
  /** Swaps the elements of this `Tuple`.
   * @return a new Tuple where the first element is the second element of this Tuple and the
   * second element is the first element of this Tuple.
   */
  def swap: Tuple2[T2,T1] = Tuple2(_2, _1)

}
```
Github 링크: [https://github.com/scala/.../Tuple2.scala#L24](https://github.com/scala/scala/blob/v2.13.1/src/library/scala/Tuple2.scala#L24)
- ```trait Product2``` 확장한다.
- 상속받은 ```trait ProductN``` 에서 추상화된 메서드 ```_N``` 을 생성자로 구현함.

## 엘러먼트 접근하기
### 방법 1)
- 각 요소들은 _1, _2, ... 와 같은 이름을 갖는다.
```scala
#!/usr/bin/env scala


println(ingredient._1)  // Sugar
println(ingredient._2)  // 25
```
### 방법 2)
- ```class TupleN```이 상속받은 ```trait ProductN```에서 구현된 ```productElement``` 메서드 사용
- 구현된 ```productElement``` 메서드는 ```trait Product```에서 추상화 돼있다.
```scala
#!/usr/bin/env scala


println(ingredient.productElement(0)  // Sugar
println(ingredient.productElement(1)  // 25
```

## 튜플에서의 패턴 매칭
- 하나의 튜플은 패턴 매칭을 사용하여 분리 가능
```scala
#!/usr/bin/env scala


val (name, quantity) = ingredient

println(name)  // Sugar
println(quantity)  // 25
```
```name```의 추론된 타입은 ```String```  
```quantity```는 ```Int```

### 튜플을 패턴 매칭한 또 다른 예제
- ```for``` comprehension
```scala
#!/usr/bin/env scala


val numPairs = List((2, 5), (3, -7), (20, 56))
for ((a, b) <- numPairs) {
  println(a * b)
}
```

- ```foreach``` comprehension
```scala
#!/usr/bin/env scala


val planets = 
  List(("Mercury", 57.9), ("Venus", 108.2), ("Earth", 149.6), 
       ("Mars", 227.9), ("Jupiter", 778.3))
  
planets.foreach{
  case ("Earth", distance) =>
    println(s"Our planet is $distance million kilometers from the sun")
  case _ =>
}
```

## Tuple과 case class
- 때로는 튜플과 케이스 클래스 중 무엇을 사용할지 고민한다.
- 케이스 클래스는 이름이 있는 엘러먼트들을 갖는다. 그 엘러먼트의 이름들은 어떤 종류의 코드에서 가독성을 높일 수 있다.
- 위의 foreach문 예제에선 튜플보다 케이스 클래스로 정의하는 것이 더 나을 수도 있겠다.
```scala
#!/usr/bin/env scala


case class Planets(name: String, distance: Double)
```
