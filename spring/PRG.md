## Post 방식으로 저장한 데이터를 새로고침 한다면?  

Post 방식으로 회원 저장, 상품 저장과 같은 기능을 구현한다. 하지만 이때 문제점이 있다.  
Post 방식으로 마무리를 하면 사용자가 새로고침 버튼을 누를 때마다 저장이 계속된다.   
이를 해결하기 위한 방법으로 PRG 방식을 사용한다.  

## PRG Post/Redirect/Get   

입력한 데이터를 Post 방식으로 가져와 저장을 하고 redirect로 다른 url을 return 하여 다시 Get 방식으로 다른 url을 매핑한다.  
이렇게 하면 사용자가 새로고침 버튼을 누르더라도 Get 방식으로 매핑한 url 페이지만 보여주고 저장이 계속 되지 않는다.  

```
    @PostMapping("/add") // Post
    public String addItemV6(Item item, RedirectAttributes redirectAttributes) {
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());  // redirect
        redirectAttributes.addAttribute("status", true);
        return "redirect:/basic/items/{itemId}";  // return 후 Get
    }
```
