# 믹스인 클래스 컴포지션
- 새로운 클래스를 정의할 때 클래스의 새로운 멤버 정의(즉, 슈퍼클래스와 비교할 때 변경된 부분)를 재사용
- python의 믹스인 처럼 다중 상속이라고 생각하면 편할 것 같다.(?)

아래는 Iterator를 추상화한 예제이다.
```scala
#!/usr/bin/env scala


abstract class AbsIterator {
  type T
  def hasNext: Boolean
  def next(): T
}
```

이어서, Iterator가 반환하는 모든 항목에 주어진 함수를 적용해주는 ```foreach```메서드로 ```AbsIterator```를 확장한 믹스인 클래스를 살펴보자.  
믹스인으로 사용하기 위해선 ```trait```이어야 한다. (```trait```이 java의 ```interface```느낌이라고 생각한다면, 쉽게 연결된다. java도 인터페이스는 다중 상속하니까는)
```scala
#!/usr/bin/env scala


trait RichIterator extends AbsIterator {
  // T 타입을 인수로 취하고 return Unit인 함수를 인수로 취하고 return Unit인 foreach 메서드 
  def foreach(f: T => Unit): Unit = { while (hasNext) f(next()) }
}
```

다움은 주어진 문자열의 캐릭터를 차례로 반환해주는 콘크리트 이터레이터
```scala
#!/usr/bin/env scala


class StringIterator(s: String) extends AbsIterator {
  type T = Char
  private var i = 0
  def hasNext = i < s.length()
  def next() = {
    val ch = s charAt i
    i += 1
    ch
  }
}
```

```RichIterator```와 ```StringIterator```를 하나의 클래스로 합치고 싶다면?  
두 클래스 모두 코드가 포함된 멤버 구현이 있기 때문에 단일상속과 인터페이스 만으론 불가능.  
이러한 상황에서 ```믹스인 클래스 컴포지션```을 활용하자.  
아래는 주어진 문자열에 포함된 모든 캐릭터를 한 줄로 출력해주는 테스트 프로그램이다.
```scala
#!/usr/bin/env scala


object StringIteratorTest {
  def main(args: Array[String]): Unit = {
    class Iter extends StringIterator("Scala") with RichIterator
    val iter = new Iter
    iter foreach println
  }
}
```
```main``` 함수의 ```Iter``` 클래스는 부모인 ```StringIterator와 ```RichIterator```를 ```with``` 키워드를 사용해 ```믹스인 컴포지션```해서 만들어졌다. 첫 번째 부모를 ```Iter```의 슈퍼클래스라 부르며, 두 번째 부모를(더 이어지는 항목이 있다면 이 모두를) ```믹스인```이라고 한다.
