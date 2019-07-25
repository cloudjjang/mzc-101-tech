# 실습 : Introduction to Amazon EC2 Auto Scaling

## LAB Overview:
이 실습에서는 Auto Scaling 기능을 사용하여 지정한 조건에 따라 Amazon EC2 인스턴스를 자동으로 시작하는 방법을 학습합니다. Auto Scaling 서비스가 자동으로 대체 인스턴스를 생성하는 동안 실행중인 인스턴스를 종료하고 감시하여 Auto Scaling 서비스를 테스트 합니다.

이 실습에서는 AWS Management Console을 사용하여 Amazon S3 기본 사항을 설명합니다.

## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* Auto Scaling 시작 구성 생성하기
* Auto Scaling 그룹 생성하기
* Auto Scaling 인프라 테스트하기
* Auto Scaling 실행 결과 확인하기


## Start Lab:

크롬 웹 브라우저에 새 탭을 만들고 다음 주소를 https://aws.amazon.com/ko/ 입력합니다. 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61853660-64a1ef00-aef7-11e9-9ba7-cbc1c8700781.png)
을 클릭하고 AWS 계정 사용자 정보를 이용해서 로그인 합니다.

<br>

## 작업 1 : 시작 구성 만들기

**1. AWS 관리 콘솔에서 서비스를 클릭하고 EC2를 선택합니다. 또는 검색창에 EC2를 검색하고 선택해도 됩니다.**

**2. 왼쪽 탐색 패널에서 AUTO SCALING 아래에 “Auto Scaling 그룹” 을 클릭합니다. (해당 메뉴가 보이지 않는 경우 스크롤을 내려서 확인해야 할 수도 있음)**

**3. ![image](https://user-images.githubusercontent.com/48195985/61853712-8a2ef880-aef7-11e9-9e97-59a115a6ca5f.png)을 클릭합니다.**

**4. Auto Scaling 그룹 생성 페이지가 열립니다. 우측 하단에  ![image](https://user-images.githubusercontent.com/48195985/61853742-987d1480-aef7-11e9-954e-b91466d2521a.png)
버튼을 클릭합니다.**

**5. 만약 아래 페이지가 보이는 경우 시작구성에 “새 시작 구성 생성”을 클릭합니다.**
> ![image](https://user-images.githubusercontent.com/48195985/61853774-aaf74e00-aef7-11e9-8670-6e1262605dd0.png)

**6. 시작 구성 생성 페이지에 빠른 시작 탭에 AMI 이미지를 선택하는 화면이 나오면 Amazon Linux 2 AMI ~ 행에서 ![image](https://user-images.githubusercontent.com/48195985/61853812-c1050e80-aef7-11e9-91a0-fd4e96961128.png)을 클릭합니다.**

> 다음으로 인스턴스 유형을 선택하라는 메시지가 나타납니다.

> 인스턴스를 시작할 때 인스턴스 유형은 인스턴스에 할당 될 하드웨어를 판별합니다. 각 인스턴스 유형은 서로 다른 컴퓨팅, 메모리 및 스토리지 기능을 제공하며 이러한 기능을 기반으로 인스턴스 패밀리로 그룹화 됩니다.

**7. t2.micro 인스턴스를 선택합니다.**

**8. ![image](https://user-images.githubusercontent.com/48195985/61853866-d9752900-aef7-11e9-8f1a-102b3d7cfe8a.png)을 클릭한 후 다음을 입력합니다.**
* 이름 : Lab-Configuration
* ![image](https://user-images.githubusercontent.com/48195985/61853907-e98d0880-aef7-11e9-8bec-cc49ee3f5c47.png)
를 클릭해서 확장합니다.

**9. IP 주소 유형 선택항목에서 다음처럼 “모든 인스턴스에 퍼블릭 IP 주소 할당”을 선택합니다.**
> ![image](https://user-images.githubusercontent.com/48195985/61853928-f4479d80-aef7-11e9-97fd-41bfce88191d.png)

**10. ![image](https://user-images.githubusercontent.com/48195985/61853956-0590aa00-aef8-11e9-9011-4426e9e172ba.png)
를 클릭합니다.**

**11. ![image](https://user-images.githubusercontent.com/48195985/61853969-0cb7b800-aef8-11e9-89b9-b3795d3bff54.png)
을 클릭합니다.**
* 보안 그룹 할당 : **새 보안 그룹 생성** 선택
* 보안 그룹 이름 : **MySecurityGroup**
*  ![image](https://user-images.githubusercontent.com/48195985/61853993-1fca8800-aef8-11e9-9580-35ca618d54f1.png)
클릭
* 유형 : **HTTP** 선택
* 원본 : **위치무관** 선택 확인 후 ![image](https://user-images.githubusercontent.com/48195985/61854001-278a2c80-aef8-11e9-8a2e-0bbe09ef82a9.png)를 클릭 한다.

**12. ![image](https://user-images.githubusercontent.com/48195985/61854024-37a20c00-aef8-11e9-8bd2-378fb6e0d250.png)
을 클릭한다.**

**13. 기존 키 페어 선택 또는 새 키 페어 생성 페이지에서 다음을 수행합니다.**
* “**키 페어 없이 계속**”을 선택합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61854099-5bfde880-aef8-11e9-9a0a-edec46481c47.png)..... 클릭합니다.

> ![image](https://user-images.githubusercontent.com/48195985/61854146-746e0300-aef8-11e9-90d4-6212deb1a3d7.png)

**14. ![image](https://user-images.githubusercontent.com/48195985/61854183-83ed4c00-aef8-11e9-8f2a-a3628af7dc81.png)
을 클릭합니다.**

<br>

## 작업 2 : Auto Scaling Group 생성하기

**15. “Auto Scaling 그룹 세부 정보 구성” 페이지를 설정합니다.**
* 그룹 이름 : **Lab-Group**
* 그룹 크기 : 시작 개수 : **1** 인스턴스
* 네트워크 : **기본값 선택**
* 서브넷 : **첫 번재 서브넷 선택**

> 위와 같은 설정하면 1개의 인스턴스로 Auto Scaling 그룹이 구성됩니다.

**16. 화면 우측 아래에 ![image](https://user-images.githubusercontent.com/48195985/61854811-ded37300-aef9-11e9-9bfa-20bd53b17e48.png)
을 클릭합니다.**

> 기본적으로 Auto Scaling 그룹은 초기크기로 유지합니다. 즉 하나의 인스턴스가 항상 작동하도록 유지 합니다. 인스턴스가 실패하면 자동으로 대체 됩니다.


**17. Auto Scaling 그룹 생성 페이지에서 ![image](https://user-images.githubusercontent.com/48195985/61854873-fa3e7e00-aef9-11e9-811b-ff281e062fb4.png)가 선택되었는지 확인하고 ![image](https://user-images.githubusercontent.com/48195985/61855053-4ee1f900-aefa-11e9-97c9-3af2a8e9f1cf.png)를 클릭합니다.**

**18. 화면 우측 아래의  ![image](https://user-images.githubusercontent.com/48195985/61855075-5a352480-aefa-11e9-9acd-a4422d3dc38f.png)
버튼을 클릭합니다.**

**19. ![image](https://user-images.githubusercontent.com/48195985/61855099-64efb980-aefa-11e9-9ae3-1992deca354d.png)를 클릭합니다.**


<br>

## 작업 3 : Auto Scaling 그룹 확인하기

Auto Scaling 그룹을 생성했으므로 그룹이 EC2 인스턴스를 시작했는지 확인합니다.
왼쪽 메뉴창에 인스턴스를 클릭합니다.

**20. 1개의 동작중인 인스턴스를 확인할 수 있습니다. 왼쪽에 ![image](https://user-images.githubusercontent.com/48195985/61855193-96688500-aefa-11e9-8d7e-0aecc0030e5e.png)를 체크 합니다.**

**21. 인스턴스 상태가 아래와 같이 running 상태인지 확인합니다. 상태검사 열은 인스턴스의 상태 검사 결과를 표시합니다.**
> ![image](https://user-images.githubusercontent.com/48195985/61855209-9ff1ed00-aefa-11e9-83e7-624fc7efa888.png)



<br>

## 작업 4 : 자동 크기 조정 테스트

Auto Scaling에 대해 자세히 알아보려면 다음 실습을 진행합니다. Auto Scaling 그룹의 최소 크기는 1개의 인스턴스로 설정되어 있습니다. 따라서 실행중인 인스턴스를 종료하는 경우 Auto Scaling이 이를 대체할 새 인스턴스를 시작합니다.

**22. 인스턴스 ID (i-0c3256efa32434ffa23와 유사합니다.)항목의 ![image](https://user-images.githubusercontent.com/48195985/61855193-96688500-aefa-11e9-8d7e-0aecc0030e5e.png)를 체크 합니다.**

**23. "작업" 버튼을 클릭후 “인스턴스 상태” -> “종료”를 누릅니다.**
> ![image](https://user-images.githubusercontent.com/48195985/61855307-ce6fc800-aefa-11e9-8df7-dfcb0fef8092.png)


**24. 인스턴스 종료 창이 뜨면 ![image](https://user-images.githubusercontent.com/48195985/61855362-e3e4f200-aefa-11e9-9c0e-f137543c3bdc.png)를 클릭합니다.
인스턴스 상태가  ![image](https://user-images.githubusercontent.com/48195985/61855385-ee9f8700-aefa-11e9-8de5-191994bf2858.png)
로 변경됩니다.**

**25. 화면 우측 상단의 ![image](https://user-images.githubusercontent.com/48195985/61855408-fbbc7600-aefa-11e9-9db2-0ca32168e325.png)
(새로고침) 아이콘을 클릭합니다. 아래 인스턴스가 추가될 때 까지 여러번 클릭합니다.** 
> (아래 이미지처럼 자동으로 1개의 인스턴스가 시작됩니다.)\
> ![image](https://user-images.githubusercontent.com/48195985/61855466-155dbd80-aefb-11e9-9ec9-3b059668ad5c.png)


> Auto Scaling 그룹의 기본 재사용 대기 시간은 300초(5분)이므로 활동기록이 표시되는데는 5분 정도 걸립니다. (왼쪽 Auto Scaling 그룹 메뉴 클릭 -> Lab-Group 선택 후 아래 “활동기록”탭 클릭)

> ![image](https://user-images.githubusercontent.com/48195985/61855526-2a3a5100-aefb-11e9-94ff-378be9023353.png)


**26. 인스턴스를 종료하고 실습을 마칩니다.**
