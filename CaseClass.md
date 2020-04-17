# 케이스 클래스
- 기본적으로 불변
- 패턴 매칭을 통해 분해가능
- 레퍼런스가 아닌 구조적 동등성으로 비교
- 초기화와 운영이 간결!

아래는 추상 상위 클래스 ```Notification```과 세개의 특정 Notification 타입들(case class Email, SMS, VoiceRecording으로 구현됨)로 구성된 Notification 타입 계층구조를 위한 예제이다.
```scala
#!/usr/bin/env scala


abstract class Notification
case class Email(sourceEmail: String, title: String, body: String) extends Notification
case class SMS(sourceNumber: String, message: String) extends Notification
case class VoiceRecording(contactName: String, link: String) extends Notification
```

케이스 클래스를 인스턴스화 하는 것은 쉽다.(new 키워드 x)
```scala
#!/usr/bin/env scala


val emailFromJohn = Email("john.doe@mail.com", "Greetings From John!", "Hello, World!")
```

생성자 파라미터들은 public 값으로 다뤄지며,
```scala
#!/usr/bin/env scala


val title = emailFromJohn.title
println(title)  // prints "Greetings From John!"
```

필드를 직접 수정할 수 없고(생성자에 var키워드를 쓰면 가능, 권장x),
```scala
#!/usr/bin/env scala


emailFromJohn.title = "test"  // 컴파일시 에러! 기본적으로 val로 할당됨
```

대신에, ```copy```메서드를 사용해서 복사본을 만들 수 있다.
```scala
#!/usr/bin/env scala


val editedEmail = emailFromJohn.copy(title = "wow!", body = "so coooool")
println(emailFromJohn)  // prints "Email(john.doe@mail.com, Greetings From John!, Hello World!)"
println(editedEmail)  // prints "Email(john.doe@mail.com, wow!, so coooool!)"
```

모든 케이스 클래스에 대해 컴파일러는  
구조적 동등성 비교를 위한 ```equals```와 ```toString``` 메서드를 생성한다.
```scala
#!/usr/bin/env scala


val firstSms = SMS("123", "test!")
val secondSms = SMS("123", "test!")

if (firstSms == secondSms) println("They are equal!")
// same As
// if (firstSms.equals(secondSms)) println("They are equal!")
// if (firstSms equals secondSms ) println("They are equal!")

println(firstSms)

------------------------------------------------------
Output:

They are equal!
SMS(123, test!)
```

패턴매칭을 사용해보자.  
아래는 어떤 Notification 타입을 받느냐에 따라 다른 메세지가 출력되는 메서드
```scala
#!/usr/bin/env scala


def showNotification(notification: Notification): String = {
  notification match {
    case Email(email, title, _) =>
      "You got an email from " + email + " with title: " + title
    case SMS(number, message) =>
      "You got an SMS from " + number + "! Message: " + message
    case VoiceRecording(name, link) =>
      "you received a Voice Recording from " + name + "! Click the link to hear it: " + link
  }
}

val someSms = SMS("123", "Are you there?")
val someVoiceRecording = voiceRecording("Tom", "voicerecording.org/id/123")

println(showNotification(someSms))
println(showNotification(someVoiceRecording))

--------------------------------------------------------------------------------------------
Output:

You got an SMS from 123! Message: Are you there?
you received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

아래는 패턴매칭에 if 방어구문을 사용한 예제이다.
```scala
#!/usr/bin/env scala


def showNotificationSpecial(notification: Notification, specialEmail: String, specialNumber: String): String = {
  notification match {
    case Email(email, _, _) if email == specialEmail =>
      "You got an email from special someone!"
    case SMS(number, _) if number == specialNumber =>
      "You got an SMS from special someone!"
    case other =>
      showNotification(other) // nothing special, delegate to our original showNotification function   
  }
}

val SPECIAL_NUMBER = "555555"
val SPECIAL_EMAIL = "jane@mail.com"
val someSms = SMS("12345", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")
val specialEmail = Email("jane@mail.com", "Drinks tonight?", "I'm free after 5!")
val specialSms = SMS("55555", "I'm here! Where are you?")

println(showNotificationSpecial(someSms, SPECIAL_EMAIL, SPECIAL_NUMBER))
println(showNotificationSpecial(someVoiceRecording, SPECIAL_EMAIL, SPECIAL_NUMBER))
println(showNotificationSpecial(specialEmail, SPECIAL_EMAIL, SPECIAL_NUMBER))
println(showNotificationSpecial(specialSms, SPECIAL_EMAIL, SPECIAL_NUMBER))

-------------------------------------------------------------------------------------------
Output:

You got an SMS from 12345! Message: Are you there?
you received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
You got an email from special someone!
You got an SMS from special someone!
```

스칼라 프로그래밍에 있어서, 보통 model/group 데이터에 case class를 사용하도록 권장된다.  
이것은 더욱 표현적이거나 유지보수가능한 코드를 작성할때 도움이 된다.
- 불변성은 그것들이 언제 어디서 수정되는지 신경쓸 필요가 없게 한다.
- 값을 통한 비교는 인스턴스들을 primitive value 처럼 비교가능하게 만든다.(참조를 통한 비교인지에 대한 불확실성 제거)
- 패턴매칭은 로직의 분기를 심플하게 만들어주어, 적은 버그 및 가독성 높은 코드로 이어진다.
