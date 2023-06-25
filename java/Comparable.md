# Comparable

Comparable 인터페이스에는 compareTo 한가지 메소드가 존재합니다.  
compareTo는 단순 동치성 비교에 더해 순서까지 비교할 수 있으며, 제네릭합니다.    
Comparable을 구현한 객체들의 배열은 손쉽게 정렬할 수 있습니다.   
따라서 알파벳, 숫자, 연대 같이 순서가 명확한 클래스를 작성한다면 Comparable 인터페이스를 구현하는 것을 고려해야합니다.    

비교를 활용하는 컬렉션인 TreeSet, TreeMap과 검색/정렬 알고리즘을 활용하는 유틸 클래스 Collections, Arrays가 있습니다.   
따라서 equals 메소드와 마찬가지로 반사성, 대칭성, 추이성과 같은 compareTo 규약을 잘 지켜야지 이러한 클래스에서        
제대로 동작할 수 있습니다.    

## compareTo 메소드, equals 메소드의 동치성

compareTo 메소드로 수행한 동치성 테스트의 결과가 equals 메소드의 결과가 같도록 해야합니다.   
BigDecimal 클래스를 예시로 들면 HashSet에서는 equals 메소드로 비교를 하여 1.0과 1.00이 다른 값이지만    
TreeSet에서는 compareTo 메소드로 비교를 하여 1.0과 1.00이 같은 값이 됩니다.   
이러한 문제가 발생하므로 동치성의 결과가 같도록 정의해야합니다.     

## Comparable 작성

Comparable은 타입을 인수로 받는 제네릭 인터페이스이므로 compareTo 메소드의 인수 타입은 컴파일 타임에 정해집니다.    
다음은 Comparable을 구현한 Member 클래스이고 compareTo 메소드를 오버라이딩한 예시입니다.    

### Member
```
public class Member implements Comparable<Member> {

    private String name;

    private int age;

    private float height;

    private float weight;

    ...

    @Override
    public int compareTo(Member member) {
        int result = Integer.compare(age, member.getAge());
        if (result == 0) {
            result = Float.compare(height, member.getHeight());
            if (result == 0) {
                result = Float.compare(weight, member.getWeight());
            }
        }
        return result;
    }
}
```

다음으로 Comparator를 이용해서 정적 메소드를 사용하여 조금 더 간결하게 표현한 방법입니다.    
약간의 성능 저하가 있지만 코드가 훨씬 깔끔해집니다.    

### MemberWithComparator

```
public class MemberWithComparator implements Comparable<MemberWithComparator> {

    private String name;

    private int age;

    private float height;

    private float weight;

    ...

    private static final Comparator<MemberWithComparator> COMPARATOR =
            Comparator.comparingInt((MemberWithComparator m) -> m.age)
                    .thenComparingDouble(m -> m.height)  // float는 double용을 이용해 수행할 수 있다
                    .thenComparingDouble(m -> m.weight);

    @Override
    public int compareTo(MemberWithComparator m) {
        return COMPARATOR.compare(this, m);
    }
}
```

또한 Comparator는 아예 Comparable이 구현되지 않은 클래스를 정렬할 때 정의하여 사용할 수 있습니다. 


```
Collections.sort(members, Comparator.comparingInt(Member::getAge)
                .thenComparingDouble(Member::getHeight)
                .thenComparingDouble(Member::getWeight));
```

## 정리

순서를 고려해야 하는 값 클래스를 작성한다면 꼭 Comparable 인터페이스를 구현하여, 그 인스턴스들을 쉽게 정렬하고,   
검색하고, 비교 기능을 제공하는 컬렉션과 어우러지도록 해야 한다.        
compareTo 메소드에서 필드의 값을 비교할 때 <와 > 연산자는 쓰지 말아야한다.    
그 대신 박싱된 기본 타입 클래스가 제공하는 정적 compare 메소드나 Comparator 인터페이스가 제공하는 비교 생성자 메소드를 사용하자.    

[아이템 14 - 예제 코드](https://github.com/320Hwany/EffectiveJava/tree/main/src/main/java/effective/item14)      
[아이템 14 - 학습 테스트](https://github.com/320Hwany/EffectiveJava/tree/main/src/test/java/effective/item14)     
