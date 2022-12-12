## 스트림을 사용하는 이유

스트림은 자바 8에서 추가한 람다를 활용할 수 있는 기술이다. 자바 8 이전에는 for, foreach로 요소를 하나씩 꺼내어 다루었지만  
스트림을 사용하면 코드의 양을 줄여 간결하게 표현할 수 있다. 즉 배열과 컬렉션을 함수형으로 표현할 수 있는 것이다.  
또한 병렬처리를 할 수 있다는 장점이 있다.  

## 스트림 사용방법

스트림 사용 방법은 3가지로 나눌 수 있다.  
1. 스트림 생성하기 : 스트림 인스턴스 생성한다
2. 가공하기 : filter, map 등을 활용해 원하는 데이터로 가공한다
3. 결과 만들기 : 최종적으로 결과를 만든다

이때 스트림은 지연 평가가 되어 모든 연산이 최종 연산이 될 때 수행된다. 또한 스트림은 단 한번의 반복문을 처리할 수 있다.  
스트림에서는 소비 개념을 사용하기 때문에 한번 사용하면 다시 접근할 수 없다.  

## 스트림 사용 예시

먼저 가공하는 과정없이 스트림 생성하고 결과 만드는 과정을 알아보자
```
@Test
void test1() {
    List<Integer> list = new ArrayList<>(Arrays.asList(1,2,3));
    list.stream()
            .forEach(a-> System.out.println(a)); // 각 요소인 1,2,3 출력
}
```
    
다음으로 스트림을 가공하는 과정을 살펴보자 map, filter, sorted를 사용하였고  
map은 특정 조건에 해당하는 값으로 변환해주고 filter는 조건에 따라 필터링 해주고 sorted는 정렬해준다. 
```
@Test
void test2() {
    List<String> list = new ArrayList<>(Arrays.asList("b", "a", "ccc", "dddd", "eeeee"));

    // map : 특정 조건에 해당하는 값으로 변환해준다.
    list.stream()
            .map(String::toUpperCase)
            .forEach(System.out::println);

    System.out.println("=================");

    // filter : 조건에 따라 걸러내는 작업
    list.stream()
            .filter(a -> a.length() < 3)
            .forEach(System.out::println);

    System.out.println("=================");

    // sorted : 요소들을 정렬해줌
    list.stream()
            .sorted()
            .forEach(System.out::println);
}
```

마지막으로 조금 더 복잡한 예를 알아보자  

먼저 foreach로 작성하면 다음과 같다
```
public int calculateForPay(List<OrderItems> orderItemsList) {
    int price = 0;
    for (OrderItems orderItems : orderItemsList) {
        Item item = orderItems.getItem();
        price += item.calculatePrice();
    }
    return price;
}
```
다음은 스트림을 사용한 방법이다. 먼저 스트림을 생성한 후 map으로 item 들을 가져온 후  
다시 Item에서 calculatePrice한 결과의 합을 가져왔다.  
```
public int calculateForPay(List<OrderItems> orderItemsList) {
    return orderItemsList.stream().map(OrderItems::getItem)
            .mapToInt(Item::calculatePrice)
            .sum();
}
```

출처 : https://futurecreator.github.io/2018/08/26/java-8-streams/
