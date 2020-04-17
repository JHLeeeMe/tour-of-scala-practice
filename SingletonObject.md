# 싱글톤 객체 (object)
- 정확히 하나의 인스턴스를 가진 클래스
- 참조될때 lazy하게 생성됨
- 로컬 값이나 클래스에 둘러졌을 때 lazy val과 똑같다 보면 됨
- As a top-level, object는 싱글톤 이다.

## Define
- object는 하나의 값이다.
- 클래스의 정의와 비슷하지만 ```object``` 키워드를 쓴다.
```scala
#!/usr/bin/env scala


object Box
```

하나의 메서드를 가진 object
```scala
#!/usr/bin/env scala


package logging

object Logger {
  def info(message: String): Unit = println(s"INFO: $message")
}
```
이제 ```info``` 메서드는 어디서든 import할 수 있다.

다른 패키지에서 ```info```를 사용해보자.
```scala
#!/usr/bin/env scala


import logging.Logger.info

class Project(name: String, daysToComplete: Int)

class Test {
  val project1 = new Project("TPS Reports", 1)
  val project2 = new Project("Website redesign", 5)
  info("Created projects")  // INFO: Created projects
}
```

## Companion objects(동반 오브젝트)
- ```object```와 같은 이름의 ```class```를 ```companion object```
- ```class```와 같은 이름의 ```object```를 ```companion class```라고 한다.
- 동반자 끼리는 서로의 ```private members```에 접근이 가능하다.
- 동반 클래스로 인스턴스화 되지 않은 값이나 메서드를 동반 오브젝트에서 사용하자.
```scala
#!/usr/bin/env scala


import scala.math._

case class Circle(radius: Double) {
  import Circle._
  def area: Double = calculateArea(radius)
}

object Circle {
  private def calculateArea(radius: Double): Double = 
    Pi * pow(radius, 2.0)
}

val circle1 = Circle(5.0)
circle1.area
```
```class Circle```에는 ```area```라는 고유한 멤버를 가지고 있으며,  
```object Circle```싱글톤에는 ```calculateArea```라는 모든 인스턴스에서 사용 가능한 메서드가 있다.

또한 동반 오브젝트는 factory methods를 포함할 수 있다.
```scala
#!/usr/bin/env scala


class Email(val username: String, val domainName: String)

object Email {
  def fromString(emailString: String): Option[Email] = {
    emailString.split('@') match {
      case Array(a, b) => Some(new Email(a, b))
      case _ => None
    }
  }
}

val scalaCenterEmail = Email.fromString("scala.center@epfl.ch")
scalaCenterEmail match {
  case Some(email) => println(
    s"""Registered an email
       |Username: ${email.username}
       |Domain name: ${email.domainName}
     """.stripMargin)
  case None => println("Error: could not parse email")
}
```
```object Email```은 ```fromString``` 팩토리를 포함하며, String값을 받아 ```Email``` 인스턴스를 생성한다.  

### 주의)
- 동반자들 끼리는 같은 파일 안에 정의되어야한다. REPL에서 정의할때는 같은 라인에 정의하던지 ```;```로 이어붙이던지 ```:paste```모드를 쓰라는것같은데 잘 모르겠음.
