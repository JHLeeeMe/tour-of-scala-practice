# Trait
- Class간에 인터페이스와 필드를 공유하는데 사용 (like interface in java8)
- class와 object는 trait을 확장 가능
- trait을 인스턴스화 할 수 없으므로 매개 변수가 없다.

## 정의
- 가장 단순한 trait 정의
```scala
#!/usr/bin/env scala


trait HairColor
```
- 제네릭 타입과 추상 메서드로 특히 유용
```scala
#!/usr/bin/env scala


trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}
```
```trait Iterator[A]```를 확장하려면 ```A```타입 지정과 ```hasNext```, ```next``` 메서드를 구현해야함.

## Trait 사용하기
- ```extends``` 예약어를 사용하여 확장  
- ```override``` 예약어로 Trait의 추상 멤버를 구현
```scala
#!/usr/bin/env scala


trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}

class IntIterator(to: Int) extends Iterator[Int] {
  private var current = 0
  
  override def hasNext: boolean = current < to
  override def next(): Int = {
    if (hasNext) {
      val t = current
      current += 1
      t
    } else 0
  }
}


val iterator = new IntIterator(10)
iterator.next()  // 0
iterator.next()  // 1
```
이 ```Iterator``` 클래스는 상한선으로 매개변수 ```to```를 취함.  
```extends Iterator[Int]```는 ```Iterator[A]``` 트레잇을 확장한다는 의미 (like implements Interface in java)  
```next``` 메서드는 Int 값을 반환

## 서브타이핑
- 특정 트레잇이 필요한 곳에 그 트레잇의 서브타입을 대신 사용 가능
```scala
#!/usr/bin/env scala


import scala.collection.mutable.ArrayBuffer

trait Pet {
  val name: String 
}

class Cat(val name: String) extends Pet
class Dog(val name: String) extends Pet

val cat = new Cat("Sally")
val dog = new Dog("Harry")

val animals = ArrayBuffer.empty[Pet]
animals.append(dog)
animals.append(cat)
animals.foreach(pet => println(pet.name))  // Prints Harry Sally
```
```trait Pet```에는 Cat과 Dog의 생성자에서 구현된 추상 필드 ```name```이 있다.  
마지막 줄에서 ```pet.name```을 호출하고 있는데, 이것은 ```trait Pet```의 서브타입에서 구현되어야 합니다.
