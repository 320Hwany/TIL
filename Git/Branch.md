# Branch 

프로젝트를 하나 이상의 모습으로 관리해야할 때 Branch를 사용합니다.    
이번에는 local에서 branch를 관리하는 방법에 대해서 알아보겠습니다.  

## Branch 생성

다음와 같이 other-branch라는 이름의 브랜치를 생성할 수 있습니다.  


```
git branch other-branch
```  
  

switch 명령어를 사용하면 현재 가리키고 있는 branch를 변경할 수 있습니다.      

```
git switch other-branch 
```
  
  
브랜치를 생성과 동시에 이동할 수도 있습니다.  

```
git switch -c other-branch
```
 

이렇게 branch를 사용하여 분기하여 각각의 코드를 관리할 수 있습니다. 

## Branch 합치기 

이렇게 각자 관리를 하던 branch를 합쳐야 할 때 2가지 방법이 있습니다.   
merge와 rebase입니다.  

1. merge : 두 브랜치를 한 지점에서 합칩니다. 브랜치 사용내역을 남겨야 할 때 사용합니다.   
2. rebase : 한 브랜치를 다른 브랜치로 합쳐 한 줄로 만듭니다. 한 줄로 깔끔하게 정리를 할 때 사용합니다.  

각각의 사용방법에 대해서 더 알아보겠습니다.   

## merge

먼저 main 브랜치에서 한번 커밋을 한 후 그 지점에서 other-branch 브랜치를 만든후 other-branch에서는 커밋을 두번  
main에서는 커밋을 한번 했다고 하겠습니다. 

<img width="250" alt="스크린샷 2023-03-16 오전 10 57 45" src="https://user-images.githubusercontent.com/84896838/225490347-cbe38312-6a24-4749-bf08-042742f8366d.png">

이제 other-branch를 main에 merge 해보겠습니다. 
```
git merge other-branch 
```
<img width="250" alt="스크린샷 2023-03-16 오전 10 59 10" src="https://user-images.githubusercontent.com/84896838/225490227-f7ffbbf4-d8c4-4b26-bdf1-3a9bb3e519b7.png">

위와 같이 branch 내역을 그대로 남기면서 합칠 수 있습니다. merge도 하나의 커밋이므로 되돌릴 수 있습니다.

## rebase

merge에서는 main에서 other-branch로 했지만 rebase는 반대로 other-branch가 main으로 합쳐진다고 생각하면 됩니다.   
이번에도 main 브랜치에서 한번 커밋을 한 후 그 지점에서 other-branch 브랜치를 만든후 other-branch에서는 커밋을 두번   
main에서는 커밋을 한번 했다고 하겠습니다. 

<img width="250" alt="스크린샷 2023-03-16 오전 11 31 07" src="https://user-images.githubusercontent.com/84896838/225494999-91c81c2c-7e7a-4648-8829-8941c20d4604.png">

```
git rebase main
```
<img width="307" alt="스크린샷 2023-03-16 오전 11 32 31" src="https://user-images.githubusercontent.com/84896838/225495132-d71ee03b-8ea5-4e87-ac55-c05f2e0dd536.png">

위와 같이 한 줄로 정리된 모습을 볼 수 있습니다. main 브랜치의 위치를 최신으로 바꾸고 싶으면 main에서 merge를 하면 됩니다.  
```
git merge other-branch
```
<img width="250" alt="image" src="https://user-images.githubusercontent.com/84896838/225495366-0399b278-2cfe-474e-9b83-aa5f6b272462.png">

출처 : 얄팍한 코딩사전 (인프런)


