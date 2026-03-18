## 4. 어디에 담아야하는지
Colletion 및 Map 인터페이스의 이해.
- 객체를 가장 담기 좋은건 Collection과 Map인터페이스를 상속한 객체다.

1. Collection은 가장 최상위 인터페이스다.
2. Set은 중복을 허용하지 않는 집합을 처리하기 위한 인터페이스다.
3. SortedSet은 오름차순을 갖는 Set 인터페이스다.
4. List는 순서가 있는 집합을 처리하기 위한 인터페이스여서 인덱스가 있어 위치를 지정하여 찾을 수 있다. 중복을 허용하고 List인터페이스를 상속받는 클래스 중에 가장 많이 사용하는 것으로 ArrayList가 있다.
5. Queue는 여러개의 객체를 처리하기 전에 담아서 처리할 때 사용하기 위한 인터페이스이다.
6. Map은 키와 쌍으로 되어있고 중복을 허용하지않는다.
7. SortedMap 키를 오름차순으로 정렬해주는 Map의 인터페이스이다.

> 먼저 Set에 대해 알아보자. Set은 중복이 없는 집합 객체를 만들 때 유용하다.<br>

- HashSet데이터를 해쉬 테이블에 담는 클래스로 순서 없이 저장된다.
- TreeSet red-black이라는 트리에 데이터를 담는다. 값에 따라 순서가 정해지고, 동시에 정렬을 한다 값을 담음과 동시에, 성능상 HashSet보다 느리다.
- LinkedHashSet 해쉬 테이블에 데이터를 담는데, 저장된 순서에 따라 순서가 결정된다.

**List에 대해 알아보자**
- 배열의 확장판이라고 보면 된다. 자동으로 구현한 클래스를 담을 수 있는 크기가 증가된다.

- ArrayList: Vector와 비슷하지만 동기화 되어있지 않다.
- Vetor: 객체 생성시에 크기를 지정할 필요가 없는 배열 클래스다.
- LinkedLis: ArrayList와 동일하지만 Queue 인터페이스를 구현해서 FIFO를 수행한다.

**Hash도 많이 있다.**
- Hashtable: 데이터를 해쉬 테이블에 담는다. 내부 관리 객체가 테이블에 동기화되어있다.
- HashMap: 데이터를 해쉬 테이블에 담는 클래스다. null을 허용하고, 동기화 되어있지 않다.
- TreeMap: red-black 트리에 데이터를 담는다. TreeSet과 다른점은 키에 의해 순서가 정해진다.
- LinkedHashMap: HashMap과 거의 동일하고 이중 연결리스트 방식을 쓴다.

이후에 Queue등도 나와있다.

### 어떤 Set이 제일 빠를까?
책에는 예시 코드로 Set을 비교한다. 하지만 소스코드가 없어서 말로 대신하겠다.

결과적으로
- HashSet : 375 마이크로초
- TreeSet : 1249 마이크로초
- LinkedHashSet : 378 마이크로초 

테스토코드도 HashSet 과 HashSetWithinitialSize는 성능이 비슷하게 나왔다.

- 다음 소스코드2

결과적으로
- HashSet : 26 마이크로초
- TreeSet : 35 마이크로초
- LinkedHashSet : 16 마이크로초 

두번째 소스코드에서는, LinkedHashSet이 나왔고 나머지는 그대로 순차적으로 나왔다.
- Set은 여러 데이터를 넣어두고 해당 데이터가 존재하는지를 확인하는 용도로도 많이 사용된다.

- generateRandomSetKeysSwap()메서드를 사용하면 데이터 개수만큼 불규칙적인 키를 뽑아 낼 수 있다. 다음에는 비순차적으로 데이터를 뽑는 SetContains를 만들어보자.

- 대충 이번 소스코드에서도 같은 결과이다.

### List 중에서는 어떤게 빠를까? 

결과이다. 단위는 마이크로초이다. 소스코드는 생략

- ArrayList : 28
- Vector : 31
- LinkedList : 40

두번째 소스코드이다. 데이터를 가져오는 시간이다.

- ArrayList : 4
- Vector : 105
- LinkedList : 1512

세번째 소스코드이다.

- ArrayList : 4
- Vector : 105
- LinkedList : 1512
- LinkedListPeek : 0.16

### Map에서는 어떤게 빠를까? 

Map은 대부분의 클래스가 동일하지만 TreeSet이 가장 느리다.
- JDK를 만드는 사람들이 시간이 남아서 여러가지 클래스를 만들었을까? 아니다.
<br>각 클래스는 용도가 다 다르고 성능이 빠른 클래스가 필요하다면 얘기는 다르지만, <br>적합한 클래스를 사용하는게 가장 좋다.

고민 하기 싫을땐
- HashSet
- ArrayList
- HashMap
- LinkedList

> 사용하는 목적에 맞는데, 해당 메서드의 성능이 잘 나오지 않을경우 JMH를 사용하여 성능 측정을 직접 해보는걸 추천한다.