## Comparable 

새로운 객체를 생성하고 그것을 다른 곳에서 Arrays로 정렬을 하고 싶다면 interface인 Comparable을 상속받아야 한다.  

```
public class Card implements Comparable<Card>{
           ...
}
```
이렇게 Comparable을 상속받으면 compareTo() 메소드를 @Override해서 구현해야한다.  

## Comparator

위에서 정의된 Card의 정렬 방법과 다르게 정렬하고 싶다면 interface인 Compartor을 상속받으면 된다.  
```
public class CardComparator implements Comparator<Card>{
           ...
}
```
이렇게 Comparator을 상속받고 compare() 메소드를 @Override해서 구현해야한다.
```
Arrays.sort(deck, new CardComparator());
```
이렇게 정렬 방법을 바꿀 수 있다.
