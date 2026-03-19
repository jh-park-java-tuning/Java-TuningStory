## 클래스 정보, 어떻게 알아낼 수 있나?

reflection 관련 클래스들 자바에는 reflection이라는 패키지가 있다. 
<br>패키지에 있는 클래스들을 사용하면 JVM 로딩 클래스와 메서드를 읽어올 수 있다.

### Class 클래스
Class 클래스는 클래스에 대한 정보를 얻을 때 사용하기 좋고, 생성자는 따로 없다.

- String getName(): 클래스의 이름을 리턴한다.
- Package getPackage(): 클래스의 패키지 정보를 패키지 클래스 타입으로 리턴한다.
- Field[] getFields(): public 으로 선언된 변수 목록을 Field클래스 배열 타입으로 리턴한다.
- Field getField(String name): public으로 선언된 변수를 Field클래스 타입으로 리턴한다.
- Field[] getDeclaredFields(): 해당 클래스에서 정의된 변수 목록을 Field클래스 배열 타입으로 리턴한다.
- Field getDeclaredField(String name): name과 동일한 이름으로 저장된 변수를 Field클래스 타입으로 리턴한다.
- Method[] getMethods(): public으로 선언된 모든 메서드 목록을 Method클래스 배열 타입으로 리턴한다. 해당 클래스에서 사용 가능한 상속받은 메서드도 포함된다.
- Method getMethod(String name, class..): 지정된 이름과 매개 변수 타입을 갖는 메서드를 Method 클래스 타입으로 리턴한다.
- Method[] getDeclardMethods(): 해당 클래스에서 선언된 모든 메서드 정보를 리턴한다.
- Method getDeclaredMethod(String name,class..): 이름과 매개변수 타입을 갖는 해당 클래스에서 선언된 메서드를 Method 클래스 타입으로 리턴한다.

다 적어보려고 했는데 너무 많아서 그만 적겠다..

현재 클래스의 이름을 알려면 이렇게 적으면된다.<br>
```java
String currentClassName=this.getClass().getName();
```

이후에 Method 클래스, Field 클래스가 나와있지만, 생략하겠다.

### reflection 클래스를 잘못 사용한 사례
일반적으로 다음과 같이 Class 클래스를 많이 사용한다.

```java
this.getClass().getName()
```

이 방법을 사용한다고 해서 성능에 많은 영향을 미치지는 않는다.<br>
다만 메서드를 호출할 때 Class 객체를 만들고 그 객체의 이름을 가져오는 메서드를 수행하는 시간과 메모리를 사용할 뿐이다.

```java
public String checkClass(Object src){
    if(src.getClass().getName().equals("java.math.BigDecimal")){
        //나머지 생략
    }
}
```

다른곳에서 실제 구현되어있는 코드다. 해당 객체 클래스 이름을 알아내기 위해서 메서드를 호출하였다. <br>이렇게 사용할 경우 응답속도에 그리 많은 영향을 주지는 않지만, 많이 사용하면 필요없는 시간을 낭비하게 된다.


```java
public String checkClass(Object src){
    if(src instanceof java.math.BigDecimal){
        //나머지 생략
    }
}
```

이렇게 하면 더욱 간단해진다. JMH로 성능 비교를 해보자

- instanceof 사용, 0.167 마이크로초
- Reflection 사용, 1.022 마이크로초

이렇게 간단하지만 성능차이가 6배나 난다. 어떻게 보면 시간으로 봤을때 큰 차이는 발생하지 않지만, 이런부분이 모여 큰 차이를 만들어서 매번 생각하면서 코딩하는것이 좋다.