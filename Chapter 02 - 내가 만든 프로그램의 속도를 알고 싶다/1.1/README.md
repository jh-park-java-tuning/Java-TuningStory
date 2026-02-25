## 내가 만든 프로그램의 속도를 알고 싶다.

내가 만든 프로그램의 구간별 응답속도는 몇일까?<br>
프로파일링 툴을 사용하면된다. APM툴도 같이 사용해야한다. 

- APM 툴을 프로파일링 툴과 비교하면 프로파일링 툴은 개발자용 툴이고, APM 툴은 운영용 툴이라고 할 수 있다. <br>

### 프로파일링 툴
- 소스 레벨의 분석을 위한 툴이다.
- 애플리케이션의 세부 응답 시간까지 분석할 수 있다.
- 메모리 사용량을 객체나 클래스, 소스의 라인 단위까지 분석할 수 있다.
- 가격이 APM툴에 비해서 저렴하다.
- 보통 사용자수 기반으로 가격이 정해진다.
- 자바 기반의 클라이언트 프로그램 분석을 할 수 있다.

### APM 툴
- 애플리케이션의 장애 상황에 대한 모니터링 및 문제점 집단이 주 목적이다.
- 서버의 사용자 수나 리소스에 대한 모니터링을 할 수 있다.
- 실시간 모니터링을 위한 툴이다.
- 보통 CPU 수를 기반으로 가격이 정해진다.
- 자바 기반의 클라이언트 프로그램 분석이 불가능하다.

> 더 많은 차이가 있지만, 이정도면 특징을 파악했을거라고 생각한다.

- 프로파일링 툴은 대부분 느린 메서드, 느린 클래스를 찾는 것을 목적으로 한다.
- APM 툴은 목적에 따라 용도가 상이하다. APM툴은 시스템 모니터링 및 운영에 강하다.
- 어떤 목적으로 사용할것인가를 잘 생각해야한다.

**프로파일링 툴이 기본적으로 제공하는것들은 무엇이 있을까?**<br>
각 툴이 제공하는 기능은 다양하고 상이하다. 응답 시간 프로파일링과 메모리 프로파일링 <br>
기능을 기본적으로 제공한다.

- 응답시간 프로파일링 기능<br>
응답시간을 측정하기 위함이다. 소스 라인단위로 응답 속도를 측정할 수도 있다. <br>
응답시간 프로파일링을 할 때 보통 CPU 시간과 대기 시간, 이렇게 두가지 시간이 제공된다.

- 메모리 프로파일링 기능<br>
잠깐 사용하고 GC의 대상이 되는 부분을 찾거나, 메모리 부족 현상이 발생하는 부분을 찾기 위함이다.<br>
클래스 및 메서드 단위의 메모리 사용량이 분석된다.

> CPU 시간과 대기 시간이라는것은 뭘까?<br>
> 하나의 메서드, 한 라인을 수행하는데 소요시간은 무조건 CPU 시간과 대기시간으로 나뉜다.
> <br> CPU 시간은 CPU를 점유한 시간을 의미하고, 대기 시간은 CPU를 점유하지 않고 대기하는 시간을 말한다.
> <br> 따라서 CPU 시간과 대기 시간을 더하면 실제 소요 시간이 된다. CPU 시간은 툴에 따라 스레드 시간으로 표시되기도 한다.

**보통 툴을 사용하면 속도 측정에 대해 간단한 답이 나온다.**<br>
더 간단하게 측정할 수 있는 방법은 없을까? 가장 간단한 방법은 System 클래스에서 제공하는 방법을 사용하는것이다.

### System 클래스
- 모든 시스템 클래스는 static으로 되어있다.
- 그 안에서 생성된 in, out, err과 같은 객체들도 static으로 선언되어있다. 생성자도 없다.
- 시스템은 우리가 객체를 생성할 수 없으며, System.XXX 방식으로 사용해야한다.

**어떤 메서드가 유용할까?**<br>

```java
static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length);
```

예제를 봐보자,

```java
public class ArrayCopyExample {
    public static void main(String[] args) {
        String[] source = {"AAA", "BBB", "CCC", "DDD", "EEE"};
        String[] destination = new String[3]; // [null, null, null]

        // source의 2번 인덱스부터 2개(CCC, DDD)를
        // destination의 1번 인덱스부터 복사
        System.arraycopy(source, 2, destination, 1, 2);

        for (String s : destination) {
            System.out.println(s);
        }
    }
}
```
- 위 예제는 System 클래스의 arraycopy 메서드를 사용하여 source 배열의 일부를 destination 배열로 복사하는 예제이다. <br>
- 여기서 만약 2가 아닌 3으로 하거나, 이상으로 지정하면 ArrayIndexOutOfBounds Exception이 발생한다. <br>

> JVM에서 설정할 수 있는 것은 크게 두가지로 나뉜다. 하나는 속성값, 다른 하나는 환경값이다.
> <br> 속성은 Properties, 환경은 env로 생각하면 된다.

- static Properties getProperties() : 시스템 속성값을 반환하는 메서드이다. <br>
- static String getProperty(String key) : 시스템 속성값을 반환하는 메서드이다. <br>
- static String getProperty(String key, String defaultValue) : key에 저장된 자바 속성값을 받아온다.<br>
- static void setProperties(Properties props) : 시스템 속성값을 설정하는 메서드이다. <br>
- static void setProperty(String key, String value) : 자바 속성에 있는 지정된 key의 값을 value 값으로 변환한다.

외에도 네이티브 라이브러리 로딩이나, 사용하면 안될 자바 코드에 대하여 설명한다.

### System.currentTimeMillis와 System.nanoTime

- static long currentTimeMillis() : 현재 시간을 ms로 리턴한다.<br>
ms는 1/1000초를 의미한다. 이 책은 별도의 지정이 없으면 ms단위로 이야기한다. <br>

> 책에는 결과가 상세하게 나와있지만, 코드를 직접 쳐야하고, 결과값을 첨부하기엔 너무 길어서 생략한다. <br>
> 31 ~ 33페이지에 자세히 설명되어있다. <br>

- 사용중인 자바 버전이 JDK 5 이상이라면 System.nanoTime()을 사용하는것이 좋다. <br>
- 성능 측정은 여러가지 방법이 있다. 메서드 말고도 전문 측정 라이브러리를 사용하는 방법도 좋다.

**라이브러리**
- JMH
- Google Caliper
- JUnitPerf
- JUnitBench
- Contiperf

이 책에서는 Caliper를 사용하려고 했지만, 안정적이지 않다고 판단해 JMH라는 툴을 사용한다.
JMH라는 툴을 사용해서 성능 측정이 가능하다.

- 나노초, 밀리초차이가 발생하는 것이 문제가 되지않는다고 생각하지말자, <br>
쌓이고 쌓이면 1초, 10초, 100초가 될 수 있다.