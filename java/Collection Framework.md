# Collection Framework

자바에서 Collection Framework를 구성하고 있는 주요 인터페이스에는 List, Set, Map이 있습니다.   

## List

순서가 있는 데이터의 집합으로 데이터의 중복을 허용합니다. 배열과 유사하지만 추가할 때마다 자동으로 boundary를 늘려줍니다.  
즉 동적배열이라고 할 수 있습니다.  

### 구현 클래스 

```
List<Integer> arrayList = new ArrayList<>();
List<Integer> linkendList = new LinkedList<>();
List<Integer> vector = new Vector<>();
```

1. ArrayList

ArrayList는 배열입니다. 따라서 데이터 조회는 매우 빠릅니다. 하지만 삽입, 삭제가 많을 때에는 배열을 새로 만들고 데이터를   
옮기기 때문에 LinkedList에 비하여 속도가 느립니다.

2. LinkedList

데이터 조회는 ArrayList보다 느립니다. 하지만 삽입, 삭제시에는 그 부분만 링크를 끊고 연결하면 되기 때문에 ArrayList보다 빠릅니다.  

3. Vector

Vector에는 동기화가 걸려있기 때문에 멀티 쓰레드에서 사용할 수 있습니다. Vector를 개선한 것이 ArrayList입니다.   
Vector는 기존 코드와의 호환을 위해서 아직 남아있으므로 다른 클래스를 사용하는게 성능이 더 좋습니다.  

## Set

순서가 없으며 데이터 중복을 허용하지 않습니다. 객체 내부에서 중복된 데이터를 허용하지 않으려면 Object 클래스의 
equals, hashcode를 재정의해야 합니다.  

### 구현 클래스

```
Set<Integer> hashSet = new HashSet<>();
Set<Integer> treeSet = new TreeSet<>();
```

1. HashSet

중복된 값을 허용하지 않고 입력한 순서를 보장하지 않으며 null 값을 허용합니다. 값 존재 유무를 파악할 때 사용합니다.   
HashSet은 HashMap을 사용해서 구현되어 있습니다.   

2. TreeSet

순서가 있는 HashSet으로 이진 트리 구조로 만들어져 있습니다. 순서에 맞게 저장하기 위해서는 Comparable을 구현해야 합니다.  

## Map

key, value 쌍으로 데이터를 저장하며 key는 중복될 수 없고 value는 중복 저장이 가능합니다.  

### 구현 클래스

```
Map<String, String> hashMap = new HashMap<>();
Map<String, String> hashTable = new Hashtable<>();
Map<String, String> concurrentHashMap = new ConcurrentHashMap<>();
```

1. HashMap

해시 알고리즘을 사용하여 많은 양의 데이터를 검색할 때 검색 속도가 매우 빠릅니다.  
하지만 동기화 문제를 보장하지 않기 때문에 멀티 쓰레드에서는 사용하면 안됩니다.    
key, value에 null 값을 허용할 수 있습니다.  

2. HashTable

HashMap과 같은 구조이지만 동기화를 보장하기 때문에 멀티 쓰레드 환경에서 사용합니다.    
동기화를 보장하기 위해 메소드를 호출하기 전에 쓰레드 간 동기화 락을 겁니다.    
모든 메소드에 synchronized가 붙어있습니다. key, value에 null 값을 허용하지 않습니다.     

3. ConcurrentHashMap

HashTable과 마찬가지로 동기화를 보장하기 때문에 멀티 쓰레드 환경에서 사용합니다.   
동기화를 보장하기 위해 락을 걸지만 HashTable과 다르게 모든 메소드에 synchronized를 붙이지 않고    
일부에만 붙여 멀티 쓰레드 환경에서 성능을 향상 시킵니다.    
예를들면 검색작업에 대해서는 락을 걸지 않습니다.   

참고한자료 

https://newwisdom.tistory.com/110   
https://rongscodinghistory.tistory.com/44





