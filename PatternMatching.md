# 패턴 매칭
- Java의 ```switch```문과 비슷하며, 더 강력하다.
- ```if/else```문을 대신할 수 있다.

## Syntax
- 하나의 값과 ```match```키워드, 그리고 최소 하나의 ```case```절이 있어야 함.
```scala
#!/usr/bin/env scala


import scala.util.Random

val x: Int = Random.nextInt(10)

x match {
  case 0 => "zero"
  case 1 => "one"
  case 2 => "three"
  case _ => "other"
}
```
```val x```는 0~10 사이의 랜덤 Int값.  
네 가지 케이스가 있다.  
마지막 ```_``` 케이스는 "catch all"을 의미(0, 1, 2를 제외한 모든 가능한 ```Int``` 타입의 값)

- Match 식은 하나의 값을 가진다.
```scala
#!/usr/bin/env scala


def matchTest(x: Int): String = x match {
  case 1 => "one"
  case 2 => "two"
  case _ => "other"
}
matchTest(1)  // one
matchTest(33)  // other
```
위의 코드에서 Match 식은 ```String```타입 값을 리턴한다.(모든 case가 String을 리턴하므로)  
그러므로 함수 ```matchTest```도 리턴값으로 String 타입의 값을 취한다.

## 케이스 클래스의 활용
- 케이스 클래스는 특히 패턴매칭이 유용하다.
```scala
#!/usr/bin/env scala


abstract class Notification

case class Email(sender: String, title: String, body: String) extends Notification

case class SMS(caller: String, message: String) extends Notification

case class VoiceRecording(contactName: String, link: String) extends Notification
```
위 코드에는  
```Notification``` 추상 클래스를 구현한 콘크리트 Notification 타입의 케이스 클래스(```Email```, ```SMS```, ```VoiceRecording```)가 있다.  
아래 코드에서 이 케이스 클래스들로 패턴매칭을 해보자.
```scala
#!/usr/bin/env scala


def showNotification(notification: Notification): String = {
  notification match {
    case Email(sender, title, _) =>
      s"You got an email from $sender with title: $title"
    case SMS(number, message) =>
      s"You got an SMS from $number! Message: $message"
    case VoiceRecording(name, link) =>
      s"You received a Voice Recording from $name! Click the link to hear it: $link"
  }
}
val someSms = SMS("12345", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")

println(showNotification(someSms))  // You got an SMS from 12345! Message: Are you there?
println(showNotification(someVoiceRecording))  // you received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

## 패턴 가드
- ```if <boolean expression>```을 사용해 cases의 표현력을 상승시킨다.
```scala
#!/usr/bin/env scala


def showImportantNotification(notification: Notification, importantPeopleInfo: Seq[String]): String = {
  notification match {
    case Email(sender, _, _) if importantPeopleInfo.contains(sender) =>
      "You got an email from special someone!"
    case SMS(number, _) if importantPeopleInfo.contains(number) =>
      "You got an SMS from special someone!"
    case other =>
      showNotification(other) // nothing special, delegate to our original showNotification function
  }
}

val importantPeopleInfo = Seq("867-5309", "jenny@gmail.com")

val someSms = SMS("123-4567", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")
val importantEmail = Email("jenny@gmail.com", "Drinks tonight?", "I'm free after 5!")
val importantSms = SMS("867-5309", "I'm here! Where are you?")

println(showImportantNotification(someSms, importantPeopleInfo))  // You got an SMS from 123-4567! Message: Are you there?
println(showImportantNotification(someVoiceRecording, importantPeopleInfo))  // You received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
println(showImportantNotification(importantEmail, importantPeopleInfo))  // You got an email from special someone!

println(showImportantNotification(importantSms, importantPeopleInfo))  // You got an SMS from special someone!
```
위 코드의 ```case Email(sender, _, _) if importantPeopleInfo.contains(sender)```절은,  
```sender```가 importantPeopleInfo의 리스트에 속해있을때 매칭된다.

## 타입 매칭
```scala
#!/usr/bin/env scala


abstract class Device

case class Phone(model: String) extends Device {
  def screenOff = "Turning screen off"
}

case class Computer(model: String) extends Device {
  def screenSaverOn = "Turning screen saver on..."
}

def goIdle(device: Device) = device match {
  case p: Phone => p.screenOff
  case c: Computer => c.screenSaverOn
}
```
위 코드에서 ```def goIdle```함수는 ```Device```의 콘크리트 클래스에 따라 결과값이 다르다.  
타입 매칭은 Match 식의 결과값으로 메서드를 취할 때 유용하다.  
타입의 첫 글자를 식별자로 적는게 관례이다.(이 경우 ```p```, ```c```)

## Sealed classes
- ```trait```과 ```class```는 ```sealed```로 마크될 수 잇다.
- 모든 서브타입이 같은 파일에 선언되어야 한다는 것을 의미한다. (모든 서브타입이 알려져 있다는 것을 의미)
```scala
#!/usr/bin/env scala


sealed abstract class Furniture
case class Couch() extends Furniture
case class Chair() extends Furniture

def findPlaceToSit(piece: Furniture): String = piece match {
  case a: Couch => "Lie on the couch"
  case a: Chair => "Sit on the chair"
}
```
```catch all``` case를 쓰지 않아도 된다는 이유로 유용하다.

## Notes
- 스칼라의 패턴 매칭은 케이스 클래스를 통해 표현된 대수 타입에 가장 유용하다.

## Details
Link: [Scala Book - Match Expressions](https://docs.scala-lang.org/overviews/scala-book/match-expressions.html)
