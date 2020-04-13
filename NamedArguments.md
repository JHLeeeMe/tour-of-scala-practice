# 이름을 지정한 파라미터

메서드와 함수를 호출할 땐 다음과 같이 명시적으로 변수의 이름을 사용할 수 있다.
```scala
def printName(first: String, last: String) = {
  println(first + " " + last)
}

printName("John", "Smith")  // "John Smith"
printName(first="John", last="Smith")  // "John Smith"
printName(last="Smith", first="last")  // "John Smith"
```

모든 파라미터에 이름이 붙어 있는 한 순서는 중요치 않다.  
이 기능은 기본 파라미터 값과 잘 어울려 동작한다.
```scala
def printName(first: String = "John", last: string = "Smith") = {
  println(first + " " + last)
}

printName(last = "Jones")  // "John Jones"
```

