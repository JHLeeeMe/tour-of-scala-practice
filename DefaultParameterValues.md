# 기본 파라미터 값

Scala는 파라미터에 기본 값을 부여 가능  
Java의 경우에는 기본 값을 제공하기 위해 수 많은 메서드를 오버로드 하는 상황이 발생, 이는 특히 생성자의 경우 그러하다.

### Java의 경우
```java
#!/usr/bin/env java


public class HashMap<K, V> {
    // 기본 크기와 로드팩터
    public static final int DEFAULT_CAPACITY = 16;
    public static final float DEFAULT_LOAD_FACTOR = 0.75;

    // Map을 받는 생성자
    public HashMap(Map<? extends K, ? extends V> m);

    // 기본 크기와 기본 로드팩터 생성자
    public HashMap();

    // 크기와 로드팩터를 파라미터로 받는 생성자들
    public HashMap(int initialCapacity);
    public HashMap(int initialCapacity, float loadFactor);
}
```

### Scala의 경우
```scala
#!/usr/bin/env scala


class HashMap[K, V](initialCapacity: Int = 16, loadFactor: Float = 0.75) {
}

// 기본 값
val m1 = new HashMap[String, Int]

// 크기 20, 로드팩터 기본값
val m2 = new HashMap[String, Int](20)

// 크기 20, 로드팩터 0.8
val m3 = new HashMap[String, Int](20, 0.8)

// 이름을 지정한 인수를 통해 로드 팩터만을 오버라이드
val m4 = new HashMap[String, Int](loadFactor = 0.8)
```

모든 기본 값에 이름을 지정한 파리미터를 활요할 수 있다.
