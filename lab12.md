# 실습 : Introduction to Amazon Aurora

## LAB Overview:
이 실습 가이드에서는 Amazon Aurora 데이터베이스를 소개합니다.

이 실습에서는 Amazon Aurora 사용에 대한 기본적인 이해를 제공합니다. Amazon Aurora 인스턴스를 만든 다음 연결하는 단계가 표시됩니다.


## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* Amazon Aurora DB 인스턴스 만들기 
* MySQL 인스턴스에 대해 미리 생성된 Amazon RDS에 연결 
* MySQL 워크벤치가 설치된 미리 생성된 Amazon EC2 인스턴스에 연결 
* 덤프 파일에서 MySQL 인스턴스에 대한 Amazon RDS(MySql)에 데이터를 로드 
* 동일한 덤프 파일에서 Amazon Aurora 인스턴스에 데이터 로드 
* Amazon Aurora 인스턴스에서 데이터 쿼리 
* MySQL 인스턴스에 대한 Amazon RDS의 데이터를 쿼리하고 결과를 비교합니다. 



## Start Lab:

https://www.qwiklabs.com/ 에서 ”Introduction to Amazon Aurora“를 검색해서 실습을 진행합니다.

<br>

**1. 상단 화면에  ![image](https://user-images.githubusercontent.com/48195985/61844251-b50a5400-aed9-11e9-92b0-cb5572b9790c.png)을 클릭하여 실습을 시작합니다.**<br>
> 그러면 LAB 리소스 프로비저닝 프로세스가 시작됩니다. LAB 리소스를 구성하는데 소요되는 예상시간이 표시됩니다. 다음을 진행하기 전에 리소스 프로비저닝이 완료될 때까지 기다려야 합니다.

<br>

**2. ![image](https://user-images.githubusercontent.com/48195985/61844296-e4b95c00-aed9-11e9-8323-6790feb05b65.png)을 클릭하면 LAB을 위한 콘솔에 자동으로 로그인이 가능합니다.**
  
<span style="color:red">※ 다른 지시가 없는 경우 Region을 변경하지 마십시오.</span>

<span style="color:red">일반적인 로그인 오류:</span>

<span style="color:red">Error : Federated login credentials</span>

<br>

![image](https://user-images.githubusercontent.com/48195985/61844451-69a47580-aeda-11e9-9b48-3814d472eb57.png)\
**만약 위 메시지를 받는 경우 :**
* 웹 브라우저를 닫고 초기 LAB으로 이동합니다.
* 30초 정도 기다린 다음 ![image](https://user-images.githubusercontent.com/48195985/61844479-880a7100-aeda-11e9-90f8-b0f1fa289f6f.png)을 클릭합니다.


<span style="color:red">**Error : You must first log out**</span>\
![image](https://user-images.githubusercontent.com/48195985/61844528-b1c39800-aeda-11e9-976e-b7a24fee70f4.png)

위 메시지가 표시되면 다른 AWS 계정에 로그인 되어 있는 창을 로그아웃 합니다.

* 빨간 박스표시된 “**Click here**”를 클릭합니다.
* 기존 웹 브라우저가 닫히면 Qwiklabs 창으로 되돌아 갑니다.
* ![image](https://user-images.githubusercontent.com/48195985/61844479-880a7100-aeda-11e9-90f8-b0f1fa289f6f.png)을 다시 클릭합니다.



<br>

## 기술 소개 :

**Amazon Aurora**
> Amazon Aurora는 고급 상용 데이터베이스의 성능과 안정성과 오픈 소스 데이터베이스의 단순성 및 비용 효율성을 결합한 완전 관리형 MySQL 호환 관계형 데이터베이스 엔진입니다. MySQL 데이터베이스를 사용하는 대부분의 기존 응용 프로그램을 변경하지 않고도 MySQL 성능의 최대 5배까지 제공합니다.

**Amazon Elastic Compute Cloud (Amazon EC2)**
> Amazon EC2는 클라우드에서 상당한 규모의 컴퓨팅 용량을 제공하는 웹 서비스입니다. 개발자가 웹 규모의 클라우드 컴퓨팅을 더 쉽게 만들 수 있도록 설계되었습니다. Amazon EC2는 새 서버 인스턴스를 프로비전하는 데 필요한 시간을 몇 분으로 단축하여 컴퓨팅 요구 사항이 변경됨에 따라 용량을 빠르게 확장할 수 있습니다.

**Amazon Relational Database Service (Amazon RDS)**
> Amazon RDS를 사용하면 클라우드에서 관계형 데이터베이스를 쉽게 설정, 운영 및 확장할 수 있습니다. 시간이 많이 소요되는 데이터베이스 관리 작업을 관리하면서 비용 효율적이고 정리 가능한 용량을 제공하므로 애플리케이션과 비즈니스에 집중할 수 있습니다. Amazon RDS는 Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL 및 MariaDB를 포함하여 선택할 수 있는 여섯개의 데이터베이스 엔진을 제공합니다.

**MySQL Workbench**
> MySQL Workbench는 데이터베이스 설계자, 개발자 및 DBA를 위한 통합 시각적 도구입니다. MySQL Workbench는 서버 구성, 사용자 관리, 백업 등을 위한 데이터 모델링, SQL 개발 및 포괄적인 관리 도구를 제공합니다. MySQL 워크 벤치는 윈도우, 리눅스와 맥에서 사용할 수 있습니다.



<br>
<br>

## 작업 1 : Amazon Aurora 인스턴스 생성
**이 작업에서는 Amazon Aurora 인스턴스를 생성합니다.**

3. AWS관리 콘솔 상단의   <img src="https://user-images.githubusercontent.com/48195985/83719113-5c672400-a671-11ea-94f6-d8a91e7a568e.png" width="60" height="20">메뉴에서 **RDS**를 클릭합니다.

4. 왼쪽 탐색창에서 **데이터베이스**를 클릭합니다.

5. 화면 우측 상단에 <img src="https://user-images.githubusercontent.com/48195985/61848523-577e0380-aee9-11e9-929a-466ce5c7c70b.png" width="120" height="25">을 클릭한 후 다음을 구성합니다.

* 데이터베이스 생성 방식 선택에서 : **표준 생성**
* 엔진 옵션 : **Amazon Aurora**
* 데이터베이스 기능 : **단일 쓰기 및 다중 잃기**
* 템플릿 : **개발/테스트**



6. **설정** 세션에서 다음을 구성합니다.

* DB 클러스터 식별자 : ```Aurora```
* 마스터 사용자 이름 : ```master```
* 마스터 암호 : ```master123```
* 암호 확인 : ```master123```



7. **DB 인스턴스 크기** 섹션에서 다음을 구성합니다.

* DB 인스턴스 클래스  : **버스터블 클래스(t클래스 포함)**을 선택합니다.
* 박스에 인스턴스 유형은 **db.t3.small** 선택


8. 가용성 및 내구성 섹션에서 **다중AZ 배포**의 경우 **Aurora 복제본 생성하지 않음**을 선택합니다.

> Amazon RDS Multi-AZ 배포는 데이터베이스(DB) 인스턴스에 대한 향상된 가용성과 내구성을 제공하므로 프로덕션 데이터베이스 워크로드에 적합합니다. Multi-AZ DB 인스턴스를 프로비전할 때 Amazon RDS는 기본 DB 인스턴스를 자동으로 만들고 다른 가용성 영역(AZ)의 대기 인스턴스에 데이터를 동기적으로 복제합니다.

> 실습 환경이므로 **다중 AZ 배포**를 수행할 필요가 없습니다.



9. **연결** 세션에서 다음을 구성합니다.

* Virtual Private Cloud(VPC) : **LabVPC**
* **추가 연결 구성**을 클릭해서 확장합니다.
* 서브넷 그룹 : **aurora**
* 퍼블릭 액세스 가능 : **아니요**
* VPC 보안 그룹 : 기존 VPC 보안 그룹 선택창에서 **DBSecurityGroup**을 선택하고 기존에 적용되어 있는 <img src="https://user-images.githubusercontent.com/48195985/83717241-32136780-a66d-11ea-88b4-3eed3b160d5e.png" width="80" height="25"> 보안그룹은 삭제합니다. 


> 서브넷은 보안 및 운영 요구 사항에 따라 리소스를 그룹화 하도록 지정하는 VPC의 IP 주소 범위의 세그먼트입니다. DB 서브넷 그룹은 VPC에서 만든 서브넷(일반적으로 비공개)의 모음이며 DB 인스턴스에 대해 지정합니다. DB 서브넷 그룹을 사용하면 CLI 또는 API를 사용하여 DB 인스턴스를 만들 때 특정 VPC를 지정할 수 있습니다. 콘솔을 사용하는 경우 사용하려는 VPC 및 서브넷만 선택할 수 있습니다.

> **aurora** 서브넷 그룹은 CloudFormation을 사용하여 랩을 시작할 때 만들어졌습니다.

> Amazon 가상 프라이빗 클라우드(Amazon VPC)를 사용하면 AWS 리소스를 정의한 가상 네트워크에서 시작할 수 있습니다. 이 가상 네트워크는 AWS의 확장 가능한 인프라를 사용할 때의 이점과 함께 자체 데이터 센터에서 운영하는 기존 네트워크와 매우 유사합니다.



10. **추가 구성**을 클릭해서 확장한 후 다음을 구성합니다.

* DB 인스턴스 식별자 : ```Aurora```
* 초기 데이터베이스 이름 : ```MyDB```


11. **암호화** 섹션에서 <img src="https://user-images.githubusercontent.com/48195985/83718571-43aa3e80-a670-11ea-8407-615aff75b7bd.png" width="120" height="20">의 체크박스를 체크해제 합니다.

> Amazon RDS DB 인스턴스에 대한 암호화 옵션을 활성화하여 Amazon RDS 인스턴스 및 스냅 샷을 암호화 할 수 있습니다.
> 유휴 상태에서 암호화 된 데이터에는 DB 인스턴스의 기본 스토리지, 자동 백업, 읽기 전용 복제본 및 스냅 샷이 포함됩니다.


12. **유지관리** 섹션에서 <img src="https://user-images.githubusercontent.com/48195985/83718644-705e5600-a670-11ea-9414-97a82fc9d333.png" width="200" height="20">을 체크해제 합니다.



13. 화면 하단으로 스크롤한 다음 <img src="https://user-images.githubusercontent.com/48195985/61848523-577e0380-aee9-11e9-929a-466ce5c7c70b.png" width="120" height="25">을 클릭합니다.


> 오로라 RDS 인스턴스를 시작 중입니다. RDS 인스턴스를 시작하는 데 최대 5분이 걸릴 수 있습니다. 그러나 다음 작업을 계속할 수 있습니다.

<br>
<br>


## 작업 2 : Amazon EC2 인스턴스에 연결
**이 작업에서는 Amazon EC2 Windows 인스턴스에 로그인합니다. 이 인스턴스는 CloudFormation을 사용하여 랩을 시작할 때 시작되었습니다. MySQL 워크벤치 클라이언트 소프트웨어가 Windows 인스턴스에 사전 설치되었습니다. 인스턴스에 로그인하면 MySQL Workbench 소프트웨어를 사용하여 데이터베이스에 연결합니다.**

14. 원도우 인스턴스에서 **Remote Desktop Connection** 클라이언트를 실행한 후 다음을 입력합니다.

    > (윈도우 서버 왼쪽 하단에 <img src="https://user-images.githubusercontent.com/48195985/83720954-101de300-a675-11ea-816d-48c3bc276bb2.png" width="60" height="25"> 돋보기 아이콘을 클릭한 후 "**remote**"를 입력하면 "**Remote Desktop Connection**" 프록그램이 검색됩니다. 클릭해서 실행합니다.)

* ![image](https://user-images.githubusercontent.com/48195985/83721219-9b977400-a675-11ea-88f6-c63074bec814.png) "Show Options"를 클릭합니다.
* Computer : 실습 지침의 왼쪽에 있는 **WindowsIP**에 명시된 IP Address를 입력합니다.
* User name : ```\administrator```
* ![image](https://user-images.githubusercontent.com/48195985/83721477-1a8cac80-a676-11ea-931f-9f054b79d58b.png)를 클릭하고 접속합니다.


15. Password 입력창에는 지침의 왼쪽에 있는 **AdministratorPassword**에 있는 값을 입력합니다.


16. "**OK**"를 클릭합니다.
    

17. "**Yes**"클릭합니다.

<br>
<br>

## 작업 3 : Amazon RDS 인스턴스에 연결
**이 작업에서는 MySQL 인스턴스뿐만 아니라 Amazon Aurora 인스턴스에 연결합니다. CloudFormation을 사용하여 실습을 시작할 때 MySQL 인스턴스가 시작되었습니다.**

### MySQL 엔드포인트 획득
> Windows 인스턴스에서 데이터베이스에 연결하려면 데이터베이스에 대한 데이터베이스 연결 세부 정보를 가져와야 합니다.

18. AWS 관리 콘솔의 <img src="https://user-images.githubusercontent.com/48195985/83719113-5c672400-a671-11ea-94f6-d8a91e7a568e.png" width="50" height="20"> 메뉴에서 **RDS**를 클릭합니다.

19. 왼쪽 탐색창에서 **데이터베이스**를 클릭합니다.

20. **mysql**을 클릭합니다.

21. **연결 및 보안** 섹션에서 **Endpoint**를 텍스트 편집기로 복사합니다.

> endpoint 주소는 다음과 유사합니다. <br>
> **_mysql.c6n9pncmlljb.ap-northeast-2.rds.amazonaws.com_**

<br>

### Amazon Aurora Endpoint 획득

22. 왼쪽 탐색창에서 **데이터베이스**를 클릭합니다.

23. **aurora** 인스턴스가 **사용가능** 상태가 될 때 까지 기다립니다.

24. **aurora** 인스턴스를 클릭합니다.

25. **연결 및 보안** 섹션에서 **Endpoint**를 텍스트 편집기에 복사합니다.
> endpoint 주소는 다음과 유사합니다. <br>
> **aurora.cluster-chpyf1ybchmg.ap-northeast-2.rds.amazonaws.com**

<br>
<br>

**Aurora Endpoint 유형**

엔드포인트는 호스트 주소와 포트가 포함된 Aurora 특정 URL로 표시됩니다. Aurora DB 클러스터에서 다음 유형의 엔드포인트를 사용할 수 있습니다.

* **클러스터 엔드포인트** : 
  >해당 DB 클러스터의 현재 기본 DB 인스턴스에 연결하는 Aurora DB 클러스터의 클러스터 엔드포인트입니다. 이 엔드포인트는 DDL 문과 같은 쓰기 작업을 수행할 수 있는 유일한 엔드포인트입니다. 따라서 클러스터 엔트포인트는 클러스터를 처음 설정할 때 또는 클러스터에 단일 DB 인스턴스만 포함될 때 연결하는 엔드포인트입니다.

각 Aurora DB 클러스터에는 하나의 클러스터 엔드포인트와 하나의 기본 DB 인스턴스가 있습니다.

insert, update, delete 및 DDL 변경 사항을 포함하여 DB 클러스터의 모든 쓰기 작업에 클러스터 엔드포인트을 사용합니다. 쿼리와 같은 읽기 작업에 클러스터 엔드포인트를 사용할 수도 있습니다.

클러스터 엔드포인트는 DB 클러스터에 대한 읽기/쓰기 연결에 대한 장애 조치 지원을 제공합니다. DB 클러스터의 현재 기본 DB 인스턴스가 실패하면 Aurora가 새 기본 DB 인스턴스로 자동으로 장애발생합니다. 장애 조치 동안 DB 클러스터는 서비스 중단을 최소화하면서 새 기본 DB 인스턴스에서 클러스터 엔드포인트에 대한 연결 요청을 계속 제공합니다.

다음 예는 Aurora MySQL DB 클러스터의 클러스터 엔드포인트를 보여 줍니다.

mydbcluster.cluster-123456789012.us-east-1.rds.amazonaws.com:3306

* **리더 엔드포인트** : 
  > Aurora DB 클러스터의 리더 엔드포인트는 해당 DB 클러스터에 사용가능한 Aurora 복제본 중 하나에 연결됩니다. 각 Aurora DB 클러스터에는 하나의 리더 엔드포인트가 있습니다. Aurora 복제본이 두 개 이상있는 경우 리더 엔드포인트는 각 연결 요청을 Aurora 복제본 중 하나로 전달합니다.

리더 엔드포인트는 DB 클러스터에 대한 읽기 전용 연결에 대한 로드 밸런싱 지원을 제공합니다. 쿼리와 같은 읽기 작업에 리더 엔드포인트를 사용합니다. 쓰기 작업에는 리더 엔드포인트를 사용할 수 없습니다.

DB 클러스터는 사용 가능한 Aurora 복제본 간에 리더 엔드포인트에 연결 요청을 배포합니다. DB 클러스터에 기본 DB 인스턴스만 포함된 경우 리더 엔드포인트는 기본 DB 인스턴스의 연결 요청을 제공합니다. 해당 DB 클러스터에 대해 하나 이상의 Aurora 복제본이 만들어지면 리더 엔드포인트에 대한 후속 연결이 복제본 간에 로드 균형이 조정됩니다.

다음 예제에서는 Aurora MySQL DB 클러스터에 대한 리더 엔드포인트를 보여 줍니다.

mydbcluster.cluster-ro-123456789012.us-east-1.rds.amazonaws.com:3306

<br>

### MySQL WorkBench 시작

26. "**원격 데스크탑**"으로 연결된 윈도우 서버 왼쪽 하단에 <img src="https://user-images.githubusercontent.com/48195985/83720954-101de300-a675-11ea-816d-48c3bc276bb2.png" width="60" height="25"> 돋보기 아이콘을 클릭한 후 "**mysql**"를 입력하면 "**MySQL Workbench 6.3 CE**" 프록그램이 검색됩니다. 

27. 검색 결과에서 **MySQL Workbench 6.3 CE**를 클릭해서 실행합니다.

    "**Unsupported Operating System**" 메시지가 나타납니다. 메시지를 무시해도 워크벤치 소프트웨어는 잘 작동합니다.

    ![image](https://user-images.githubusercontent.com/48195985/83727449-09e13400-a680-11ea-9dca-be6683468c5c.png)

<br>

28. "**Unsupported Operating System**" 경고에서  다음을 수행합니다.
  * "**Don't show this message again**"을 체크합니다.
  * "**OK**"를 클릭합니다.

<br>
<br>

### 데이터베이스에 연결
29. **MySQL Workbench 홈 화면**에서 <img src="https://user-images.githubusercontent.com/48195985/83825209-a6084b00-a713-11ea-9675-e23528e18998.png" width="200" height="25">에  **더하기** 기호를 클릭하여 새 연결을 만듭니다.
    <br>

30. "**Setup New Connection**" 창에서 다음을 입력합니다.
  * **Connection Name** : ```Aurora```
  * **Hostname** : _복사 해 놓은 Aurora의 Endpoint 주소를 입력합니다._
  * **Username** : ```master```
  * "**Store in Vault**"를 클릭합니다.
  * **Password** : ```master123```
  * "**OK**"를 클릭합니다.


31. "**Test Connection**"을 클릭합니다.
<br>

32. "**OK**"를 클릭해서 **Success** 메시지를 닫습니다.
<br>

33. "**Setup New Connection**" 창의 "**OK**"를 클릭합니다.
> 새로운 **Aurora Connection**이 표시됩니다.
<br>

34. MySQL 인스턴스에 연결하려면 위의 단계를 반복합니다. 연결 이름과 호스트 이름을 사용하여 앞서 언급한 MySQL 엔드포인트 주소를 붙여넣습니다.

> _정상적으로 완료되면 MySQL과 Aurora 연결 2개가 표시되어야 합니다._

<br>
<br>


## 작업 4 : SQL 덤프 파일을 데이터베이스로 가져오기

35. **원격 데스트탑 연결**에서 Windows 시작 버튼 옆의 돋보기 아이콘을 클릭합니다.
<br>

36. 검색 창에 **powershell**을 입력합니다.
<br>

37. 검색 결과에서 **Windows PowerShell**을 클릭합니다.
<br>

38. PowerShell에서 다음을 입력합니다.
    
```powershell
Invoke-WebRequest https://s3-us-west-2.amazonaws.com/aws-tc-largeobjects/SPLs/sharedDatabases/world.sql -OutFile c:\\Users\\Administrator\\Desktop\\world.sql
```
_이 명령은 데스크톱에 SQL 덤프 파일(world.sql)을 다운로드 합니다._

<br>
<br>


### MySQL 데이터베이스로 덤프 파일 가져오기
<br>

39. **MySQL Workbench** 응용프로그램에서 **MySQL** 연결을 클릭합니다.
<br>

40. **MANAGEMENT** 아래 **Data Import/Restore**를 클릭합니다.
> ![image](https://user-images.githubusercontent.com/48195985/83733790-04d4b280-a689-11ea-9fa8-b62cbdfa6e45.png)
<br>

41. **Import Options**에서 **Import from Self-Contained File**을 선택합니다.
<br>

42. 오른쪽 끝에 Ellpsis ![image](https://user-images.githubusercontent.com/48195985/83733978-4d8c6b80-a689-11ea-9cc9-df8a94231571.png) 버튼을 클릭합니다. <br>

43. 왼쪽 창에서 ![image](https://user-images.githubusercontent.com/48195985/83734098-83315480-a689-11ea-9476-2692a6d48a3e.png)을 클릭합니다.

44. **word** 덤프파일을 더블클릭합니다.

45. ![image](https://user-images.githubusercontent.com/48195985/83734232-acea7b80-a689-11ea-9d67-2ee7ff8ebc15.png)를 클릭합니다.

46. 왼쪽창 아래 "**SCHEMAS**" 아래 refresh 심볼을 클릭합니다.
>![image](https://user-images.githubusercontent.com/48195985/83733871-26ce3500-a689-11ea-9151-a8b0e93e4cd4.png)

아래처럼 world 스키마가 등록되어야 합니다.<br>
> ![image](https://user-images.githubusercontent.com/48195985/83734492-09e63180-a68a-11ea-9337-196d7176beef.png)

<br>

### 덤프 파일을 Aurora 데이터베이스로 가져오기

47.  Workbench 프로그램 좌측 상단에 홈 버튼을 클릭합니다.

> ![image](https://user-images.githubusercontent.com/48195985/83734885-8e38b480-a68a-11ea-8d00-bf6070795de5.png)

**그러면 아래처럼 2개의 연결이 모두 표시됩니다.<br>**
> ![image](https://user-images.githubusercontent.com/48195985/83735238-0ef7b080-a68b-11ea-98d6-6a1cd53f6d54.png)


48.  **Aurora** 연결을 클릭합니다.
> Aurora 데이터베이스에 대한 쿼리창이 표시되고  또한 화면 상단에 MySQL, Aurora 두 개의 탭이 표시 되어야 합니다. <br>
    
>![image](https://user-images.githubusercontent.com/48195985/83735512-7281de00-a68b-11ea-8657-d4331c6a7d54.png)


49.  MySQL 데이터베이스로 가져오데 사용한 것과 동일한 단계를 사용하여 world 덤프 파일을 Aurora 데이터베이스로 가져옵니다.<br>

> **아래처럼 SCHEMAS 아래 world 데이터베이스가 생성되어야 합니다.**

![image](https://user-images.githubusercontent.com/48195985/83736002-03f15000-a68c-11ea-9d00-7dafe2286b57.png)


<br>
<br>


## 작업 5 : 데이터베이스 쿼리

이 작업에서는 MySQL 데이터베이스와 Aurora 데이터베이스로 가져온 데이터베이스를 쿼리합니다.

### MySQL 데이터베이스에서 쿼리 실행

50. 화면 상단의 MySQL 탭을 클릭합니다.<br>
    
    >![image](https://user-images.githubusercontent.com/48195985/83736493-aad5ec00-a68c-11ea-8a15-f12c956bfd5b.png)


51. "**Query 1**" 탭을 클릭합니다.


52. Query 창에 다음을 입력합니다.
    ```sql
    select * from world.city where Population > 10000
    Order by CountryCode;
    ```
53. ![image](https://user-images.githubusercontent.com/48195985/83736953-52ebb500-a68d-11ea-8b6d-fa405896cdaa.png) 단추를 클릭해서 쿼리를 실행합니다.

    > _아래 이미지 처럼 쿼리에 결과로 직원 정보가 표시됩니다._

    ![image](https://user-images.githubusercontent.com/48195985/83736835-25067080-a68d-11ea-8ef6-eefb71db7c2b.png)

### Aurora 데이터베이스에서 쿼리 실행

54. 화면 상단의 **Aurora** 탭을 클릭합니다.


55. "**Query 1**"을 클릭합니다.

56. 동일한 쿼리를 실행합니다.
    ```sql
    select * from world.city where Population > 10000
    Order by CountryCode;
    ```
    > _쿼리는 동일한 결과를 출력합니다._

<br>
<br>

### 실습 종료

다음 단계에 따라 실습을 종료하고 실습환경을 평가합니다.

57. **AWS 관리 콘솔**로 돌아갑니다.


58. 네비게이션 바에서 "**awsstudent@(AccountNumber)**"를 클릭하고 **Sign Out**을 클릭합니다.


59. <img src="https://user-images.githubusercontent.com/48195985/83737959-a90d2800-a68e-11ea-8734-5a898a804dd2.png" width="70" height="25">을 클릭합니다.


60. <img src="https://user-images.githubusercontent.com/48195985/83738082-cc37d780-a68e-11ea-9a87-f353b415a38b.png" width="40" height="25">를 클릭합니다.

61. 실습에 대한 평가를 해주시고 "**Submit**"을 클릭합니다.



## 실습을 완료합니다.
