## Post 방식으로 저장한 데이터를 새로고침 한다면?  

Post 방식으로 회원 저장, 상품 저장과 같은 기능을 구현한다. 하지만 이때 문제점이 있다.  
Post 방식으로 마무리를 하면 사용자가 새로고침 버튼을 누를 때마다 저장이 계속된다.   
이를 해결하기 위한 방법으로 PRG 방식을 사용한다.   

PRG 방식은 클라이언트가 Post 방식으로 서버에게 요청을 하면 서버는 302 Found와 같은 Status Code가 담긴 HTTP 응답 메세지를   
클라이언트에게 보낸다. 클라이언트는 HTTP 응답 메세지의 header에 있는 Location의 위치를 보고 다시 Get 메소드를 이용해서   
서버에 요청한다. 그리고 서버로 부터 응답을 받는다. 이러한 과정을 거치면 마지막에 한 요청이 Get 요청이므로 새로고침을 하더라도  
그냥 그 화면만 다시 보여주면 되므로 계속 저장되는 문제를 해결할 수 있다.  

## 스프링에서의 PRG 사용 예시

입력한 데이터를 Post 방식으로 가져와 저장을 하고 redirect로 다른 url을 return 하여 다시 Get 방식으로 다른 url을 매핑한다.  
이렇게 하면 사용자가 새로고침 버튼을 누르더라도 Get 방식으로 매핑한 url 페이지만 보여주고 저장이 계속 되지 않는다.  

```
    @PostMapping("/add") // Post
    public String addItem(Item item, RedirectAttributes redirectAttributes) {
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());  // redirect
        redirectAttributes.addAttribute("status", true);
        return "redirect:/basic/items/{itemId}";  // return 후 Get
    }
```
