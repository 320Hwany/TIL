# AWS EC2

EC2는 Elastic Compute Cloud의 약자로 AWS에서 제공하는 성능, 용량 등을 유동적으로 사용할 수 있는 서버입니다.   
이제부터 EC2 인스턴스를 생성해보겠습니다. AWS 로그인 후 인스턴스 시작을 누릅니다.   

## 인스턴스 생성하기 

1. Name and Tags

사용할 이름을 지정해줍니다.   

<img width="774" alt="image" src="https://user-images.githubusercontent.com/84896838/233596192-f696549b-7bb7-426f-8df1-64fc6744bd49.png">

2. 애플리케이션 및 OS 이미지 

우선 AMI(Amazon Machine Image)를 선택해야합니다.  
AMI는 EC2 인스턴스를 시작하는 데 필요한 정보를 이미지로 만들어 둔 것을 이야기합니다.  
인스턴스라는 가상머신에 운영체제 등을 설치할 수 있게 구워 넣은 이미지입니다.   
아마존이 개발하기 때문에 지원받기 쉬운 Amazon Linux를 선택하고 프리티어로도 가능한   
Amazon Linux 2023 AMI를 선택하였습니다.  

<img width="762" alt="image" src="https://user-images.githubusercontent.com/84896838/233596553-440952e0-6d5a-498f-b2ab-8b3acf30e5f1.png">


3. 인스턴스 유형   

인스턴스 유형으로는 t2.micro를 선택하였습니다. t2는 요금 타입을 의미하고 micro는 사양을 의미합니다.  

<img width="776" alt="image" src="https://user-images.githubusercontent.com/84896838/233596378-9b4b7c80-99bc-4c54-938c-2c235a0c7e58.png">

4. 키 페어(로그인)

다음으로 키 페어를 설정해주는데 키 페어는 EC2 서버로 접속하기 위한 키라고 생각하시면 됩니다.   
이 키는 절대 잃어버리면 안되므로 잘 관리할 수 있는 디렉토리에 저장하는 것이 좋습니다.    

<img width="756" alt="image" src="https://user-images.githubusercontent.com/84896838/233598058-10852c5f-edd4-484f-8baf-1643203e8949.png">

5. 네트워크 설정

보안 그룹 이름과 보안 그룹 규칙을 설정합니다.   
SSH Secure Shell의 줄임말로 원격 호스트에 접속하기 위한 보안 프로토콜이므로  
내 IP에서만 접속할 수 있도록 설정합니다.  

<img width="768" alt="스크린샷 2023-04-21 오후 6 26 30" src="https://user-images.githubusercontent.com/84896838/233609909-7658de4e-b25e-4c36-b53b-5f1db786f05e.png">
<img width="776" alt="233600236-83e6c2a5-89bc-48eb-9d12-25982c9eb0fc" src="https://user-images.githubusercontent.com/84896838/233609516-4115f568-8d49-4947-acca-b78968026559.png">


6. 스토리지 설정

프리티어는 최대 30GB까지 가능하므로 30GB로 설정해줍니다.   

<img width="784" alt="image" src="https://user-images.githubusercontent.com/84896838/233601250-34303792-3e1d-42a6-ba3c-f7dee5c2eed4.png">  

이제 인스턴스 생성이 완료되었습니다.   
지금은 인스턴스를 중지하고 다시 시작하면 새 IP가 할당되는데 매번 변경되지 않고 고정 IP를 가지게 하려면   
탄력적 IP를 할당하면 됩니다.  


## EC2 서버에 접속하기

위 방식으로 생성한 EC2에 접속을 해보겠습니다.   

1. 위에서 생성한 pem키를 ~/.ssh 디렉토리에서 관리하기 위해 다음 명령어로 이동시키고 권한을 변경하겠습니다.   
```
mv {pem 키 이름} ~/.ssh
chmod 600 {pem 키 이름}
```

2. 편리하게 EC2 서버로 접속하기 위해 config 파일을 생성하겠습니다.   
```
vim config
```

```
HOST EC2-Test
   Hostname {탄력적 IP 주소}
   User ec2-user
   IdentityFile ~/.ssh/ec2-test.pem
```

실행 권한을 부여하겠습니다.   
```
chmod 700 ~/.ssh/config
```

이제 다음과 같은 간단한 명령어로 EC2 서버에 접속할 수 있습니다.   
```
ssh EC2-Test
```

## EC2 서버에서 설정 변경하기

EC2 서버에서 타임존, 호스트 네임을 변경해야 합니다.     
기본 서버의 시간은 미국 시간대이므로 한국 시간대를 사용하려면 변경이 필요합니다.   
EC2 서버에 접속하면 IP가 보이기 때문에 여러 서버를 돌릴 때 구분하기 위해서는 호스트 네임이 필수입니다.   

1. 한국 시간으로 변경하기

```
sudo rm /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```

2. 호스트 네임 변경하기

```
sudo vim /etc/sysconfig/network
```

```
NETWORKING=yes
HOSTNAME=localhost.localdomain
NOZEROCONF=yes
```

여기에 /etc/hosts에 변경하나 hostname을 등록합니다
```
sudo vim /etc/hosts
```
```
127.0.0.1  localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost6 localhost6.localdomain6

127.0.0.1 ec2-test
```

호스트 네임을 등록하는 방법은 다음과 같은 방법도 있습니다.  
```
sudo hostnamectl set-hostname [호스트 이름]
```

출처 : 스프링 부트와 AWS로 혼자 구현하는 웹 서비스 (이동욱)




