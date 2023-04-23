# RDS

이번 글에서는 직접 데이터베이스를 설치하지 않고 AWS에서 지원하는 모니터링, 알람, 백업, HA 구성 등의 기능을  
지원하는 클라우드 기반 관계형 데이터베이스인 AWS RDS에 대해 알아보겠습니다.   
먼저 RDS 인스턴스를 생성하겠습니다.  

## 인스턴스 생성 

1. 엔진 옵션 

엔진 옵션에는 사용할 데이터베이스, 버전을 선택하면 됩니다.   

2. 템플릿 

템플릿은 용도에 따라 선택하면 됩니다. (프리티어를 선택했습니다)   

3. 설정

설정에서는 DB 식별자, 사용자 이름, 비밀번호를 설정합니다.   
여기에 설정한 사용자 정보로 실제 데이터베이스에 접근합니다.   

<img width="770" alt="image" src="https://user-images.githubusercontent.com/84896838/233826350-b35fdd94-f489-4fbf-a50a-ab8e83d75eea.png">

4. 인스턴스 구성

선택한 데이터베이스의 옵션을 정합니다. 기본 옵션을 사용했습니다.  

5. 스토리지 

스토리지 용량을 지정하고 자동 조정 옵션은 해제하였습니다.  

<img width="751" alt="image" src="https://user-images.githubusercontent.com/84896838/233826512-c7014427-935a-4f73-aeea-9affa8203c39.png">

6. 연결  

퍼블릭 액세스 가능에 '예'로 체크합니다. 보안그룹에서 지정된 IP만 접근하도록 막을 수 있습니다.    

7. 추가 구성

데이터베이스 이름을 설정합니다. 여기서 DB 파리미터 그룹은 타임존, Character Set, Max Connection과 같은   
DB 옵션을 설정하기 위해 AWS에서 제공하는 기능입니다. 우선 기본 값으로 설정하겠습니다.   

<img width="596" alt="image" src="https://user-images.githubusercontent.com/84896838/233826673-69af70ee-93bb-4e61-b119-1ae403e733ad.png">

## 파라미터 그룹 생성

이제 파라미터 그룹을 생성해보겠습니다. RDS에서 파라미터 그룹 옵션을 들어가서 생성해줍니다.   

- time_zone   
기본적으로 UTC 시간으로 세팅되어있습니다. Asia/Seoul로 수정합니다.  

- Character Set    
다음과 같이 Utf8mb4로 변경합니다. utf8과 utf8mb4의 차이는 이모지 저장 가능 여부입니다.  

<img width="356" alt="image" src="https://user-images.githubusercontent.com/84896838/233826877-e79e4b82-860a-46a9-95de-2de2f9848cb4.png">


- max_connections    
Max Connections는 인스턴스 사양에 따라 자동으로 정해집니다.    
원하는 커넥션 수로 설정을 합니다. (150으로 설정하였습니다)    

## 데이터베이스에 접속하기

이렇게 생성한 데이터베이스에 접속을 해보겠습니다. 데이터베이스에 접속하기 위해서는 데이터베이스 보안 그룹에서 허용해주어야 합니다.   
생성한 데이터베이스의 보안 그룹을 클릭하여 인바운드 규칙 편집을 누릅니다.    
참고로 인바운드 규칙은 외부에서 안으로 들어오는 즉 로컬 PC, EC2 서버와 같은 외부에서 데이터베이스(내부)로 접근하는 규칙입니다.   
여기에서 MYSQL/Aurora 유형으로 내 IP와 EC2 서버의 보안 그룹을 추가해줍니다.     

이제 로컬 PC, EC2 서버에서 RDS에 접근할 수 있게 되었습니다.   

1. 로컬 PC

로컬 PC에서는 Workbench와 같은 GUI를 이용하면 편리합니다. 아까 설정했던 사용자 정보를 입력하여 접근합니다.  
접근한 후 생성된 콘솔창에 쿼리가 수행될 데이터베이스를 선택하는 다음 SQL을 실행합니다.  
```
use {AWS RDS 웹 콘솔에서 지정한 데이터베이스명};
```

2. EC2 서버

EC2 서버에서는 MySQL에 CLI로 접근하겠습니다. 우선 MySQL을 설치한 후 다음 명령어를 실행합니다.  
```
mysql -u {계정} -p {엔드 포인트}
```

출처 : 스프링 부트와 AWS로 혼자 구현하는 웹 서비스 (이동욱)  
