# 실습 : Creating an Amazon Virtual Private Cloud (VPC) with AWS CloudFormation

## LAB Overview:
이 실습에서는 AWS CloudFormation을 사용하여 Amazon VPC (Virtual Private Cloud)를 생성하는 방법을 보여줍니다.

AWS 사용 CloudFormation은 신뢰할 수 있고 반복 가능한 방식으로 VPC를 배포하는 훌륭한 방법입니다. CloudFormation에서 사용되는 템플릿이 배포 대상을 정확하게 보여주기 위해 문서로 사용되기 때문입니다.

AWS CloudFormation 템플릿의 둘러보기 섹션과 배포 된 리소스를 살펴 봅니다. 또한 CloudFormation을 통해 업데이트를 수행하는 방법을 배우게됩니다.


## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* Amazon VPC를 생성하는 AWS CloudFormation 템플릿 배포
* 템플릿의 구성 요소 검사
* CloudFormation 스택 업데이트
* AWS CloudFormation Designer를 사용하여 템플릿 검토
* CloudFormation 스택 삭제


## Start Lab:

크롬 웹 브라우저에 새 탭을 만들고 다음 주소를 https://aws.amazon.com/ko/ 입력합니다. 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61862588-14338d00-af09-11e9-9114-e61081e51851.png)
을 클릭하고 AWS 계정 사용자 정보를 이용해서 로그인 합니다.

<br>

## 작업 1 : AWS CloudFormation을 사용하여 스택 배포

**1. 다음 주소를 클릭해서 “http://bit.ly/32ulj2c” vpc-1.yaml 파일을 다운로드 합니다.**

> 다운로드 받은 vpc-1.yaml 파일을 코드에디터나 노트패드를 이용해서 내용을 검토합니다.

**2. AWS 관리 콘솔에서  ![image](https://user-images.githubusercontent.com/48195985/61928236-cb381300-afb2-11e9-8382-8b5c7cab5bbc.png)
메뉴에 CloudFormation을 클릭합니다.**

**3. 만약 아래와 같은 메시지창이 보아면 “지금 사용해보고 의견을 보내주십시오.”를 클릭합니다.**
> ![image](https://user-images.githubusercontent.com/48195985/61928255-d9862f00-afb2-11e9-99e1-a4501b167a7d.png)

**4. ![image](https://user-images.githubusercontent.com/48195985/61928283-e73bb480-afb2-11e9-8365-ea3d1a865af5.png)을 클릭하고 다음을 구성합니다.**

* ![image](https://user-images.githubusercontent.com/48195985/61928308-f6bafd80-afb2-11e9-97b4-69ceeea6b6ca.png)를 선택합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61928318-fe7aa200-afb2-11e9-98e2-e7ebc83ae781.png)을 클릭합니다.
* 다운로드 받은 vpc-1.yaml 파일을 선택하고 열기를 클릭합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61928331-063a4680-afb3-11e9-9fa9-917945fbfac7.png)
을 클릭합니다.

**5. “스택 세부 정보 지정” 페이지에서 다음을 구성합니다.**

* 스택 이름 : **Lab**
* ![image](https://user-images.githubusercontent.com/48195985/61928331-063a4680-afb3-11e9-9fa9-917945fbfac7.png)을 클릭합니다.

> 옵션 페이지에서는 태그, 권한 및 고급 옵션을 지정할 수 있습니다.

**6. “스택 옵션 구성” 페이지에서 스크롤을 아래로 내린 다음 ![image](https://user-images.githubusercontent.com/48195985/61928331-063a4680-afb3-11e9-9fa9-917945fbfac7.png)을 클릭합니다.**

**7. “Lab 검토” 페이지에서 스크롤을 아래로 내려 설정된 내용을 검토하고 ![image](https://user-images.githubusercontent.com/48195985/61928283-e73bb480-afb2-11e9-8365-ea3d1a865af5.png)을 클릭합니다.**

> 스택 상태가 리소스가 생성될 때까지 CREATE_IN_PROGRESS 로 표시됩니다.\
![image](https://user-images.githubusercontent.com/48195985/61928404-4c8fa580-afb3-11e9-9be7-852e63ec4769.png)

> 기다리는 동안 이벤트 탭을 클릭하고 진행상황을 모니터링 합니다.

> ※ 이벤트 내용이 보이지 않으면 브라우저 창을 키우고 버튼을 누릅니다.

**8. “스택정보” 탭을 클릭합니다.**

**9. 상태가 ![image](https://user-images.githubusercontent.com/48195985/61928436-692bdd80-afb3-11e9-882f-fc4d658e6e14.png)로 변경될 때 까지 기다립니다. 30초마다 새로고침 ![image](https://user-images.githubusercontent.com/48195985/61928464-7cd74400-afb3-11e9-86d4-f19b72f929cb.png)
을 클릭하여 상태를 업데이트 합니다.**

> 스택 상태가  ![image](https://user-images.githubusercontent.com/48195985/61928436-692bdd80-afb3-11e9-882f-fc4d658e6e14.png)이면 자원 설정이 완료되었음을 의미합니다.


**10. “리소스”탭을 클릭합니다.**

> 자원 목록이 표시됩니다. 표시된 리소스에 대한 설명은 다음 작업에서 진행합니다. 리소스를 보려면 화면을 새로고침![image](https://user-images.githubusercontent.com/48195985/61928464-7cd74400-afb3-11e9-86d4-f19b72f929cb.png)
 해야할 수 있습니다.


<br>

## 작업 2 : vpc 검사

이 작업에서는 리소스를 만든 CloudFormation 템플릿의 코드와 생성된 VPC 리소스를 검사합니다.

CloudFormation에서 만든 리소스는 다음과 같습니다.

* Amazon VPC
* Internet Gateway
* Subnet 2개
* Route Table 2개

> 이러한 리소스는 모두 하나의 가용영역(AZ) 내에 있습니다 . 가용영역(AZ)은 Region내의 격리된 위치이며 하나 이상의 데이터 센터로 구성됩니다.


**11. ![image](https://user-images.githubusercontent.com/48195985/61928535-b445f080-afb3-11e9-9d75-2974163d2d57.png)메뉴에서 VPC를 클릭합니다.**

**12. 왼쪽 상단모서리에 VPC로 필터링: 검색창을 클릭하고 출력된 VPC 리스트중 Lab VPC를 선택합니다. 이렇게 하면 VPC 콘솔에 CloudFormation에서 만든 VPC에 속한 리소스만 표시되도록 구성됩니다.**

![image](https://user-images.githubusercontent.com/48195985/61928548-c32ca300-afb3-11e9-8664-f3de78c07ba4.png)

**13. 왼쪽 탐색창에서 VPC를 클릭합니다.**

14. ![image](https://user-images.githubusercontent.com/48195985/61928585-e6efe900-afb3-11e9-9f6d-106b03b9816a.png)를 선택합니다. 

> VPC는 리소스가 서로, 선택적으로 인터넷과 통신 할 수 있게 해주는 AWS 클라우드의 분리 된 섹션입니다. Amazon EC2 인스턴스와 같은 리소스를 배포할 때는 인스턴스가 시작될 VPC를 선택해야 합니다.
> 
> 설명 탭의 IPv4 CIDR는 VPC에 할당된 IP 주소의 범위이다. 이 VPC는 10.0.0.0/16 의 CIDR을 가지며 이는 다음으로 시작하는(10.0.x.x) 모든 IP 주소를 포함함을 의미합니다. .

> 다음은 이 VPC를 만든 CloudFormation 템플릿의 코드입니다.

```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: Deploy a VPC

Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsHostnames: true
      Tags:
      - Key: Name
        Value: Lab VPC
```

> 이 코드는 YAML 형식입니다. AWS CloudFormation은 JSON 형식의 코드도 사용 할 수 있습니다. 

> 위 코드 의 Type 매개 변수는 CloudFormation에서 만드는 리소스 유형을 선언합니다. 그런 다음 Properties 섹션에서 만들 리소스에 대한 자세한 정보를 지정합니다. 

> **이 경우 다음을 정의합니다.**
>
> * CidrBlock : VPC와 관련된 IP 주소 범위입니다.
> * EnableDnsHostnames : DNS 이름을 Amazon EC2 인스턴스와 연결하도록 VPC를 구성합니다.
> * 태그 : 자원에 친숙한 이름을 추가합니다.

**15. 왼쪽 탐색창에서 “인터넷 게이트웨이”를 클릭합니다.**

> 인터넷 게이트웨이는 사용자의 VPC와 인터넷의 인스턴스 간의 통신을 가능하게 하는 수평 확장, 중복, 고가용성을 제공하는 VPC의 구성 요소입니다. 따라서 네트워크 트래픽에 가용성 위험이나 대역폭 제약을 부과하지 않습니다.

> 인터넷 게이트웨이는 인터넷 라우팅 트래픽에 대한 VPC 경로 테이블에 대상을 제공하고 공용 IPv4 주소가 할당 된 인스턴스에 대해 NAT (네트워크 주소 변환)를 수행하는 두 가지 용도로 사용됩니다.

> 다음은 인터넷 게이트웨이를 만든 CloudFormation 템플릿의 코드입니다.

```yaml
  InternetGateway:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
      - Key: Name
        Value: Lab Internet Gateway
```
> 관리 콘솔에서 인터넷 게이트웨이가 VPC에 연결되어 있음을 보여줍니다. 이것은 CloudFormation 템플릿에서 다음 코드로 수행되었습니다.

```yaml
  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref VPC
      InternetGatewayId: !Ref InternetGateway
```

> VPC 게이트웨이 연결은 VPC와이 인터넷 게이트웨이와 같은 게이트웨이 사이의 관계를 만듭니다.\
> 템플릿은 템플릿의 다른 요소를 **!Ref** 키워드로 다른 자원의 이름을 참조합니다.\
> 이렇게 하면 단순히 이름을 참조하여 서로 연결되는 리소스를 쉽게 만들 수 있습니다.

**16. 왼쪽 탐색 분할 창에서 ‘서브넷’을 클릭합니다.**

> 두 개의 서브넷이 표시됩니다.
* Public Subnet 1은 인터넷 게이트웨이를 통해 인터넷에 연결되며 공개적으로 액세스 할 수 있어야 하는 리소스를 배치합니다.
* Private Subnet 1은 인터넷에 연결되어 있지 않습니다. 이 서브넷의 모든 리소스는 인터넷을 통해 접근 할 수 없으므로 리소스 추가적인 보안을 제공합니다.

> 다음은 서브넷을 생성 한 CloudFormation 템플릿의 코드입니다.

```yaml
  PublicSubnet1:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.0.0/24
      AvailabilityZone: !Select
        - '0'
        - !GetAZs ' '
      Tags:
        - Key: Name
          Value: Public Subnet 1
PrivateSubnet1:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.1.0/24
      AvailabilityZone: !Select
        - '0'
        - !GetAZs ' '
      Tags:
        - Key: Name
          Value: Private Subnet 1
```
> 속성은 다음과 같습니다.
* VpcId 는 서브넷을 포함하는 VPC를 나타냅니다.
* CidrBlock 은 서브넷에 할당 된 IP 주소의 범위입니다.
* AvailabilityZone 은 서브넷이 위치할 가용영역을 정의 합니다.

> 가용 영역(AZ)은 !Select 라는 함수와 !GetAZs 라는 함수를 사용합니다. 이 코드는 영역 내의 가용 영역 목록을 검색하고 목록에서 첫 번째 요소를 참조합니다. 이 방법으로 템플릿은 가용성 영역을 템플릿에 하드 코딩하지 않고 런타임에 가용성 영역 목록을 검색하기 때문에 모든 영역에서 사용할 수 있습니다.

**17. 왼쪽 탐색창에서 ‘라우팅 테이블’을 클릭합니다.**

**18. Public Route Table을 선택합니다. ![image](https://user-images.githubusercontent.com/48195985/61929653-772f2d80-afb6-11e9-81ff-dbe1a09dcee2.png)**

**19. 창의 아래쪽에 있는 ‘라우팅’탭을 클릭합니다.**

> 라우팅 테이블은 트래픽을 서브넷 안팎으로 라우팅하는 데 사용됩니다. 이 경로 테이블의 구성은 다음과 같습니다.
* VPC(10.0.0.0/16)내의 트래픽의 경우 트래픽을 로컬로 라우팅합니다.
* 인터넷으로 가는 트래픽(0.0.0.0/0)의 경우 트래픽을 인터넷 게이트웨이(igw-로 표시)로 라우팅합니다.

> 다음은 Public Route Table을 생성 한 CloudFormation 템플릿의 코드입니다.

```yaml
 PublicRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
        - Key: Name
          Value: Public Route Table
```
 
> Private Route Table도 유사한 코드를 사용합니다.

> 다음은 Public Route Table 내에서 인터넷에 대한 경로를 정의한 코드입니다.

```yaml
PublicRoute:
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PublicRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway
```  
> 경로 구성은 다음과 같습니다.
> * RouteTableId 는 경로를 소유할 Route Table을 나타냅니다.
> * DestinationCidrBlock 은 이 라우팅 규칙의 IP 주소 범위를 정의합니다. 0.0.0.0/0은 인터넷으로 향하는 트래픽을 나타냅니다.
> * GatewayId 는 트래픽을 라우팅 할 Internet Gateway를 정의합니다.

> 이 Route는 Public Route Table에 대해서만 구성되어 공개 됩니다.


**20. ‘서브넷 연결’ 탭을 클릭합니다.**

> ‘Public Route Table’이 ‘Public Subnet 1’ 과 연결되어 있음을 보여줍니다. 라우팅 테이블은 여러 개의 서브넷과 연결 될 수 있으며 각 연결에는 명시적 연결이 필요합니다.

> 이 연결를 정의한 코드는 다음과 같습니다.

```yaml
PublicSubnetRouteTableAssociation1:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet1
      RouteTableId: !Ref PublicRouteTable
```

> Public Subnet 1 은 Public Route Table 과 연결되어 있음을 선언합니다.

> 리소스를 생성하는 것 외에도 CloudFormation은 생성 된 리소스에 대한 정보를 제공 할 수 있습니다.

**21.  ![image](https://user-images.githubusercontent.com/48195985/61929771-e442c300-afb6-11e9-87d3-cec218dfbeb3.png)메뉴에서 CloudFormation을 클릭합니다.**

**22. Lab 스택을 선택합니다.**

**23. ‘출력’ 탭을 클릭합니다.**

> CloudFormation 템플릿은 생성 된 리소스에 대한 정보를 반환하도록 구성되었습니다.
> * VPC 는 생성 된 VPC의 ID입니다.
> * AZ1 은 서브넷이 생성 된 가용성 영역을 보여줍니다.

> 다음은 출력을 구성한 코드입니다.

```yaml
Outputs:
  VPC:
    Description: VPC
    Value: !Ref VPC
AZ1:
    Description: Availability Zone 1
    Value: !GetAtt
      - PublicSubnet1
      - AvailabilityZone
```
> VPC 출력은 단순히 VPC에 대한 참조이며 VPC ID를 표시합니다.

> AZ1 출력은 !GetAtt 함수를 사용하여 자원의 속성을 검색합니다. 이 경우 ‘Public Subnet 1’에서 AvailabilityZone 속성을 검색하고 있습니다.

<br>

## 작업 3 : 스택 업데이트

CloudFormation 스택을 배포 한 후에는 리소스를 직접 수정하지 않고 CloudFormation을 통해 리소스를 변경해야 합니다.

이 작업에서는 다음 리소스를 정의하는 새로운 CloudFormation 템플릿으로 스택을 업데이트합니다.

![image](https://user-images.githubusercontent.com/48195985/61929893-47ccf080-afb7-11e9-9922-bdc8b028aa29.png)

Public 및 Private 서브넷이 다른 가용영역(AvaliabilityZone)에 추가 되었습니다. 시스템 장애 발생 시 고 가용성을 보장하기 위해 리소스를 여러 데이터센터(가용성 영역)에서 실행할 수 있도록 하는 것이 가장 좋습니다 .


**24. 다음 주소를 클릭하고 http://bit.ly/2JIQzC5 vpc-2.yaml 파일을 다운로드 합니다.**

**25. AWS 콘솔창의 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61929937-6501bf00-afb7-11e9-8f2e-f5856c78ccbc.png)를 클릭합니다.**
* 사전조건-템플릿 준비 아래 ![image](https://user-images.githubusercontent.com/48195985/61929973-6e8b2700-afb7-11e9-9a22-f9c58e4e7f33.png)를 선택합니다.
* 템플릿 지정 아래 ![image](https://user-images.githubusercontent.com/48195985/61929988-7945bc00-afb7-11e9-980b-ad84a8f8376d.png)를 선택합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61930001-819df700-afb7-11e9-9dfe-cd9af93e638f.png)을 클릭합니다.
* 다운로드 받은 vpc-2.yaml 파일을 선택하고 열기를 클릭합니다.

**26. ![image](https://user-images.githubusercontent.com/48195985/61928331-063a4680-afb3-11e9-9fa9-917945fbfac7.png)을 클릭합니다.**

**27. ![image](https://user-images.githubusercontent.com/48195985/61928331-063a4680-afb3-11e9-9fa9-917945fbfac7.png)을 클릭합니다.**

**28. 화면 아래 마지막까지 스크롤하고 ![image](https://user-images.githubusercontent.com/48195985/61928331-063a4680-afb3-11e9-9fa9-917945fbfac7.png)을 클릭합니다.**

**29. 화면의 아래 마지막까지 스크롤하고 변경사항을 확인한 다음 ![image](https://user-images.githubusercontent.com/48195985/61930066-aeeaa500-afb7-11e9-9512-7313653e3f41.png)을 클릭합니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61930093-c0cc4800-afb7-11e9-879e-47100b7dceee.png)
> 기다리는 동안 이벤트 탭을 클릭하고 새로고침을 클릭한 후 진행상황을 모니터링 합니다.

**30. ‘스택정보’ 탭을 클릭합니다.**

> 상태가 ![image](https://user-images.githubusercontent.com/48195985/61930156-ea856f00-afb7-11e9-8998-cdc24d7634a9.png)로 변경될 때 까지 기다립니다. 30초마다 ![image](https://user-images.githubusercontent.com/48195985/61930129-d6417200-afb7-11e9-9726-a2050b18c808.png)새로고침을 클릭하여 상태를 업데이트 합니다.

**31. ‘출력’탭을 클릭합니다.**

> 원래 가용영역과 다른 값을 가진 추가 가용영역이 표시됩니다.

**32.  ![image](https://user-images.githubusercontent.com/48195985/61929771-e442c300-afb6-11e9-87d3-cec218dfbeb3.png) 메뉴에 VPC 클릭합니다.**

**33. 왼쪽 상단모서리에 VPC로 필터링: 검색창을 클릭하고 출력된 VPC 리스트중 Lab VPC를 선택합니다. 이렇게 하면 VPC 콘솔에 CloudFormation에서 만든 VPC에 속한 리소스만 표시되도록 구성됩니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61930204-10127880-afb8-11e9-93f8-addc6249a647.png)

**34. 왼쪽 탐색창에서 ‘서브넷’을 클릭합니다.**

> 4개의 서브넷이 표시됩니다. 각각의 서브넷을 클릭하고 ‘라우팅 테이블’탭에 설정을 검사합니다.

> VPC는 고가용성을 제공하도록 업데이트 되었습니다.


<br>

## 작업 4 : CloudFormation Designer에서 스택 보기

이 작업에서는 AWS CloudFormation Designer를 사용하여 템플릿을 볼 수 있습니다.

AWS CloudFormation Designer(디자이너)는 AWS CloudFormation 템플릿을 만들고, 보고, 수정하기 위한 그래픽 도구입니다. Designer를 사용하면 드래그 앤 드롭 인터페이스를 사용하여 템플릿 리소스를 다이어그램으로 표시 한 다음 통합된 JSON 및 YAML 편집기를 사용하여 세부 정보를 편집 할 수 있습니다. 신규 또는 숙련 된 AWS CloudFormation 사용자는 AWS CloudFormation Designer를 사용하면 템플릿 리소스 간 상호 관계를 신속하게 파악하고 템플릿을 쉽게 수정할 수 있습니다.

**35. ![image](https://user-images.githubusercontent.com/48195985/61929771-e442c300-afb6-11e9-87d3-cec218dfbeb3.png) 메뉴에서 CloudaleFormation을 클릭합니다.**

**36. Lab 스택을 선택합니다.**

**37. 템플릿 탭을 클릭합니다.**

**38. 콘솔창의 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61930296-42bc7100-afb8-11e9-9671-ed25dc087fd5.png)
를 클릭합니다.**

> 창의 맨 윗부분은 템플릿에 정의된 VPC의 그래픽 개요를 제공합니다.

**39. 확대/축소 컨트롤을 사용해서 다이어그램을 검사합니다. 이미지를 드래그하여 다이어그램을 이동할 수 있습니다.**

**40. 페이지 하단에서 ‘구성요소’탭을 클릭합니다.**

**41. 위쪽의 다이어그램에서 구성요소를 클릭합니다.**

> 다이어그램의 구성요소 이미지를 클릭하면 창 아래 부분에 Resource를 정의하는 템플릿의 코드를 표시합니다.

> 화살표는 서브넷과 연력된 라우팅 테이블과 같은 리소스 간의 관계를 보여줍니다.

**42. 템플릿의 언어를 변경하려면 JSON을 클릭합니다.**
> 디자이너는 JSON과 YAML 형식간에 코드를 변환 할 수도 있습니다.

<br>

## 작업 5 : 스택 삭제

이 작업에서는 스택을 삭제하면 VPC와 해당 구성 요소가 자동으로 삭제됩니다.

**43. 화면 왼쪽 상단에 ‘닫기’링크를 클릭합니다.**

> 그러면 디자이너가 종료됩니다. 메시지가 나타나면 ‘나가기’를 클릭하고 종료합니다.

**44. Lab 스택을 선택합니다.**

4**5. 콘솔창 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61930369-78f9f080-afb8-11e9-9606-87c95afa0ca6.png)를 클릭한 후 메시지가 나타나면 ![image](https://user-images.githubusercontent.com/48195985/61930379-80b99500-afb8-11e9-9893-72a515ea802b.png)
를 클릭합니다.**

**46. ‘이벤트’탭을 클릭하여 삭제의 세부사항을 확인합니다.**

> 새로고침을 클릭한 후 진행상황을 모니터링 합니다.
> 
> 스택이 삭제되면 목록에서 사라집니다.
> 
> VPC도 삭제되었습니다.

**47.  ![image](https://user-images.githubusercontent.com/48195985/61929771-e442c300-afb6-11e9-87d3-cec218dfbeb3.png)메뉴에 VPC 클릭합니다.**

**48. 왼쪽 탐색창에서 VPC를 클릭합니다.**

> Lab VPC는 더 이상 목록에 존재하지 않습니다. 또한 관련 인터넷 게이트웨이, 서브넷 및 라우팅 테이블도 삭제되었습니다.

<br>
<br>

## 실습을 종료합니다. 
