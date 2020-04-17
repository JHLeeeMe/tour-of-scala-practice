# Multiple Parameter Lists (Currying)
- 메서드는 파라미터 목록을 여럿 정의할 수 있다.
- 정의된 파라미터 수 보다 적은 파라미터로 메서드가 호출되면, 해당 메서드는 누락된 파라미터 목록을 인수로 받는 새로운 메서드를 만든다.
```scala
#!/usr/bin/env scala


object CurryTest extends App {
  def filter(xs: List[Int], p: Int => Boolean): List[Int] ={
    if (xs.isEmpty) xs
    else if (p(xs.head)) xs.head :: filter(xs.tail, p)
    else filter(xs.tail, p)
  }

  def modN(n: Int)(x: Int)  = ((x % n) == 0)

  val nums = List(1, 2, 3, 4, 5, 6, 7, 8)
  println(filter(nums, modN(2)))
  println(filter(nums, modN(3)))
}

------------------------------------------------------------
Output:

List(2, 4, 6, 8)
List(3, 6)
```
```modN```메서드는 두 번의 ```filter```호출에서 부분적으로 사용됨.  
즉, 첫 번째 인수만이 실제로 사용됐다.  
```modN(2)```라는 구문은 ```Int => Boolean```타입의 함수를 만들기 때문에 ```filter```함수의 두 번째 인수로 사용할 수 있게 된다.  
한마디로다가 ```modN(2)```은 ```(x: Int) => ((x % 2) == 0)```함수 느낌이다.

