# 실습 : Introduction to AWS Identity and Access Management (IAM)

## LAB Overview:

AWS Identity and Access Management (IAM)는 Amazon Web Services (AWS) 고객이 AWS에서 사용자 및 사용자 권한을 관리할 수 있게 해주는 웹 서비스입니다. IAM을 사용하면 사용자, 액세스 키와 같은 보안 자격증명 및 사용자가 액세스 할 수 있는 AWS리소스를 제어하는 권한을 중앙에서 관리할 수 있습니다.

## Topics Covered:

### 이 LAB에서는 다음의 내용을 학습합니다.

* 기존 IAM 사용자 및 그룹을 확인합니다.
* 기존 그룹에 적용된 IAM 정책을 확인할 수 있습니다.
* 특정 권한이 할당된 그룹에 사용자를 추가가 할 수 있습니다.
* IAM 로그인 URL 찾기 및 사용이 가능합니다.
* 서비스 액세스 정책을 적용하고 테스트 할 수 있습니다.



### Start Lab:

1. 상단 화면에  ![image](https://user-images.githubusercontent.com/48195985/61770545-7bc5db80-ae28-11e9-882a-272837e4a332.png)을 클릭하여 실습을 시작합니다. 
   
   그러면 LAB 리소스 프로비저닝 프로세스가 시작됩니다. LAB 리소스를 구성하는데 소요되는 예상시간이 표시됩니다. 다음을 진행하기 전에 리소스 프로비저닝이 완료될 때까지 기다려야 합니다.

   ![image](https://user-images.githubusercontent.com/48195985/61770673-c8111b80-ae28-11e9-9635-3d7983d7e233.png)

                 
2. ![image](https://user-images.githubusercontent.com/48195985/61770802-19210f80-ae29-11e9-8ae9-94884d127f3e.png)을 클릭하면 LAB을 위한 콘솔에 자동으로 로그인이 가능합니다.
    > ※ 다른 지시가 없는 경우 Region을 변경하지 마십시오.

    > **<span style="color:red"> 일반적인 로그인 오류 : </span>**
    
    > **<span style="color:red"> Error : Federated login credentials</span>**


![image](https://user-images.githubusercontent.com/48195985/61775164-57233100-ae33-11e9-8a2d-10512d96ac17.png)

### 만약 위 메시지를 받는 경우 :

* 웹 브라우저를 닫고 초기 LAB으로 이동합니다.
  
* 30초 정도 기다린 다음 ![image](https://user-images.githubusercontent.com/48195985/61775254-86d23900-ae33-11e9-83c1-c8737b7364fc.png)을 클릭합니다.
    > **<span style="color:red"> Error : You must first log out </span>**
    

![image](https://user-images.githubusercontent.com/48195985/61775312-abc6ac00-ae33-11e9-880c-49b6cb1c8c71.png)

##### 위 메시지가 표시되면 다른 AWS 계정에 로그인 되어 있는 창을 로그아웃 해야합니다.

* 빨간 박스표시된 “Click here”를 클릭합니다.
* 기존 웹 브라우저가 닫히면 Qwiklabs 창으로 되돌아 갑니다.
* ![image](https://user-images.githubusercontent.com/48195985/61775254-86d23900-ae33-11e9-83c1-c8737b7364fc.png)을 다시 클릭합니다.

---
### 작업 1 : IAM Users 와 Groups 탐색하기

##### 이 작업에서는 IAM에 이미 생성된 사용자 및 그룹을 탐색합니다.

**3. AWS Management Console에서 **Services** 메뉴의 **IAM**을 클릭합니다. 또는 검색창에 IAM을 입력하고 검색된 서비스를 클릭해도 됩니다.**
   > ![image](https://user-images.githubusercontent.com/48195985/61775599-360f1000-ae34-11e9-9d4c-3505ea53e2d4.png)

**4. IAM 관리창에서 왼쪽 탐색창에서 “Users”를 클릭합니다. 다음 IAM 사용자가 생성되어 있습니다.**
   > * user-1
   > * user-2
   > * user-3
  
또한 awsstudent 사용자도 있습니다. 실습에서는 무시하셔도 됩니다.

**5. user-1을 클릭하십시오.**
   > user-1의 요약(Summary) 페이지가 보이고 “권한(Permissions)” 탭이 표시됩니다.\
   > ![image](https://user-images.githubusercontent.com/48195985/61775876-c3526480-ae34-11e9-9d96-e519f3d11f04.png)

**6. user-1에게는 현재 권한(Permissions)이 할당되어 있지 않습니다.**

**7. “그룹(Groups)”탭을 클릭합니다.**

   > user-1은 어떤 그룹에도 포함되어 있지 않습니다.

**8. “보안 자격 증명(Security credentials)”을 클릭합니다.**

   > user-1 사용자는 “콘솔 비밀번호(Console password)“가 할당되어 있습니다.

**9. 왼쪽 ”탐색창“에서 “그룹”을 클릭합니다.**

   > 다음 그룹이 미리 생성되어 있습니다.
   > * EC2-Admin
   > * EC2-Support
   > * S3-Support

**10. EC2-Support 그룹을 클릭합니다.**

    EC2-Support 그룹의 요약페이지를 보여줍니다.

**11. 권한(Permission) 탭을 클릭합니다.**
    
   > 이 그룹에는 “Amazon.com2ReadOnlyAccess“라는 AWS관리 정책이 연결되어 있습니다.  
   > AWS관리정책은 IAM 사용자 및 그룹에 할당 할 수 있는 AWS에의해 사전 제작된 정책입니다.

**12. 정책이름(Policy Name) 우측의 작업(Action) 아래 정책표시(Show Policy)를 클릭합니다.**
    
   > 정책(Policy)는 특정 AWS 리소스에 대해 허용되거나 거부되는 작업을 정의합니다. 이 정책은 EC2, Elastic Load Balancing, CloudWatch 및 Auto Scaling에 대한 정보를 나열하고 설명 할 수 있는 권한을 부여합니다. 리소스를 볼 수는 있지만 수정할 수는 없습니다.<p>
   > ![image](https://user-images.githubusercontent.com/48195985/61776216-8a66bf80-ae35-11e9-866f-9a6a8776a1ee.png)<p>
   > IAM 정책의 기본 구문은 다음과 같습니다.
   > * Effect : 사용권한을 허용할지 또는 거부할지 여부가 표시됩니다.
   > * Action : AWS 서비스(예:CloudWatch:ListMetrics)에 대해 수행할 수 있는 API호출을 저정합니다.
   > * Resource : 정책 규칙이 적용되는 엔티티의 범위를 정의합니다. (예: 특정 Amazon S3 버킷 또는 Amazon EC2 인스턴스 또는 *는 모든 리소스를 의미함)


**13.  “정책표시(Show Policy)” 창 아래 “취소(Close)”를 클릭합니다.**

**14.  왼쪽 “탐색창”에서 “그룹(Groups)”를 클릭합니다.**

**15.  S3-Support 그룹을 클릭합니다.**

   > S3-Support 그룹은 AmazonS3ReadOnlyAccess 정책이 연결되어 있습니다.

**16. 정책이름(Policy Name) 우측의 작업(Action) 아래 정책표시(Show Policy)를 클릭합니다.**

   > 이 정책에는 Amazon S3에서 리소스를 가져오고 나열할 수 있는 권한이 있습니다.

**17. “정책표시(Show Policy)” 창 아래 “취소(Close)”를 클릭합니다.**

**18. 왼쪽 탐색창에서 “그룹”을 클릭합니다.**

**19. EC2-Admin 그룹을 클릭합니다.**

   > 이 그룹은 기존 2개의 그룹과는 약간 다릅니다. “관리형 정책” 대신 “인라인 정책”이 할당되어 있습니다. 
   > 인라인 정책은 일반적으로 일회용 사용권한을 적용하는데 사용합니다.

**20. 정책이름(Policy Name) 우측의 작업(Action) 아래 정책편집(Edit Policy)를 클릭합니다.**

   > 이 정책은 Amazon EC2에 대한 정보와 인스턴스 시작 및 중지 기능을 표시할 수 있는 권한이 부여되어 있습니다.

**21. 화면 우측하단의 “취소(Close)”를 클릭하여 정책을 닫습니다.**

<br>
<br>

### Business Scenario :

이 실습의 나머지 부분에서는 이러한 사용자 및 그룹과 협력하여 다음 비즈니스 시나리오를 지원하는 사용권한을 활성화 합니다.

귀사는 Amazon Web Services를 많이 사용하고 있으며 많은 Amazon EC2 인스턴스와 많은 양의 Amazon S3 스토리지를 사용하고 있습니다. 업무 기능에 따라 새로운 직원에게 권한을 부여하고자 합니다.


   | User | in Group | Permissions |
   | ---- | -------- | ----------- |
   |user-1|S3-Support|Read-Only access to Amazon S3|
   |user-2|EC2-Support|Read-Only access to Amazon EC2|
   |user-3|EC2-Admin|View, Start and Stop Amazon EC2 instances|


<br>
<br>
<br>

## 작업 2 : 그룹에 사용자 추가하기

최근에 user-1을 Amazon S3에 대한 지원을 제공하는 역할로 채용했습니다. S3-Support 그룹에 추가하여 첨부 된 AmazonS3ReadOnlyAccess 정책을 통해 필요한 권한을 상속받습니다.

※ 이 작업중에 나타나는 “승인되지 않은” 오류는 무시할 수 있습니다. 이는 사용 권한이 제한된 랩 계정으로 인해 발생하며 랩을 완료하는데 영향을 미치지 않습니다.
<br>
<br>
### S3-Support 그룹에 user-1 사용자 추가하기
<br>

**22. 왼쪽 탐색 창에서 “그룹”을 클릭합니다.**

**23. S3-Support 그룹을 클릭합니다.**

**24. 사용자 탭을 클릭합니다.**

**25. 사용자 탭아래 “그룹에 사용자 추가”를 클릭합니다.**

**26. “그룹에 사용자 추가”창에서 다음을 구성합니다.**
   > * user-1을 선택합니다. (user-1 사용자 이름 왼쪽의 박스를 체크함)
   > * 왼쪽 아래 **“사용자 추가”** 버튼을 클릭합니다.
   > * 사용자 탭에 user-1 사용자가 추가된 것을 확인할 수 있습니다.

   > **user-2 사용자를 EC2-Support 그룹에 추가합니다.**
   > **user-2는 Amazon EC2에 대한 지원을 제공하는 역할로 채용하였습니다.**

**27.  위의 단계와 유사한 단계를 사용하여 user-2를 EC2-Support 그룹에 추가합니다.**
    
   > user-2는 이제 EC2-Support 구룹의 구성원이여 합니다.
   > user-3 사용자는 EC2-Admin 그룹에 추가합니다.
   > user-3은 EC2인스턴스를 관리하는 Amazon EC2 관리자로 채용했습니다.

**28. 위의 단계와 동일한 방법으로 user-3을 EC2-Admin 그룹에 추가하십시오.**

   > user-3은 EC2-Admin 그룹의 구성원이여야 합니다.

**29. 왼쪽 탐색창에서 그룹을 선택합니다.**

   > 각 그룹에는 각 그룹의 사용자열에 수가 1로 표시되어 있어야 합니다. (아래 참고)<p>
   > ![image](https://user-images.githubusercontent.com/48195985/61776902-ed0c8b00-ae36-11e9-82dc-52b0c54dfb33.png)


<br>
<br>

## 작업 3 : 사용자 로그인 및 테스트

이 작업에서는 각 IAM 사용자의 사용 권한을 할당합니다.
<br>

**30. 왼쪽 탐색창에서 대시보드를 클릭합니다.**

   > IAM 사용자 로그인 링크가 표시됩니다. 아래와 유사합니다.\
   > https://775362843861.signin.aws.amazon.com/console\
   > 이 링크를 사용하면 현재 사용중인 AWS 계정에 IAM 사용자로 로그인 할 수 있습니다.

**31. IAM 사용자 로그인 링크를 텍스트 편집기로 복사하십시오.**
   > ![image](https://user-images.githubusercontent.com/48195985/61777405-cdc22d80-ae37-11e9-8f54-6af8dd33ecf7.png)

**32. 웹 브라우저의 Private Windows를 엽니다. (브라우저별 참고)**

**Mozilla Firefox**
   > * 오른쪽 위에 메뉴바![image](https://user-images.githubusercontent.com/48195985/61777447-de72a380-ae37-11e9-8199-e0f6e1fb9553.png)를 클릭합니다.
   > * “새 사생활 보호 보드“ 를 클릭해서 새창을 엽니다.

**Google Chrome**
   > * 오른쪽 위에 메뉴바![image](https://user-images.githubusercontent.com/48195985/61777476-ecc0bf80-ae37-11e9-8418-6a6f04adf2b1.png) 를 클릭합니다.
   > * ”새 시크릿 창“을 클릭해서 새창을 엽니다.

**Microsoft Edge**
   > * 오른쪽  위에 생략부호 ![image](https://user-images.githubusercontent.com/48195985/61777511-f9ddae80-ae37-11e9-8836-0efff3100c58.png)
를 클릭합니다.
   > * ”새 InPrivate 창“을 클릭해서 새창을 엽니다.

<br>

**33. IAM 사용자 로그인 링크 정보를 복사해서 주소창에 붙여넣기 합니다.**

   > Amazon S3 스토리지 지원업무로 채용된 user-1 사용자로 로그인합니다.

**34. 로그인 정보:**
   > * 사용자 이름 : user-1
   > * 암호 : lab-password

**35. 서비스 메뉴에서 S3를 클릭합니다.**

**36. 버킷중 하나를 클릭하고 내용을 볼 수 있는지 확인합니다.**

   > 사용자는 IAM S3-Support 그룹에 포함되어 있기 때문에 Amazon S3 버킷 목록과 내용을 확인할 수 있는 권한이 있습니다.\
   > 이제 Amazon EC2에 액세스 할 수 있는지 테스트 합니다.

**37. 서비스 메뉴에 EC2를 클릭합니다.**

**38. 왼쪽 탐색창에서 인스턴스를 클릭합니다.**

   > 인스턴스를 볼 수 없습니다. 아래처럼 권한이 없음을 표시합니다.\
   > 사용자가 Amazon EC2에 사용 권한을 할당받지 않았기 때문입니다.

![image](https://user-images.githubusercontent.com/48195985/61777836-97d17900-ae38-11e9-8f8d-c5e1b5dc5a42.png)

**이제 Amazon EC2 지원 담당자로 채용된 user-2로 로그인 합니다.**

**39. 다음을 구성하여 AWS Management Console에 user-1 사용자 로그아웃 합니다.**

   > * 화면 상단에 user-1 사용자 정보를 클릭합니다.
   > * 로그아웃을 클릭합니다.

   > ![image](https://user-images.githubusercontent.com/48195985/61778549-ef241900-ae39-11e9-9ae7-827737b09b3b.png)

**40. 시크릿모드로 실행중인 웹 브라우저에 이전에 복사해 놓은 IAM 사용자 링크 정보를 붙여넣습니다.**

**41.  다음 정보로 로그인 합니다.**
* 사용자 이름 : user-2
* 암호 : lab-password

**42. 서비스 메뉴에서 EC2를 클릭합니다.**

**43. 왼쪽 탐색창에서 인스턴스를 클릭합니다.**

   > 읽기 전용 권한이므로 Amazon EC2 인스턴스를 볼 수 있습니다. 그러나 Amazon EC2 리소스를 변경할 수는 없습니다.<p>
   > ※ Amazon EC2 인스턴스를 볼 수 없으면 Region 올바르지 않을 수 있습니다. 화면의 오른쪽 상단에서 Region 메뉴를 선택해서 오레곤(Oregon)을 선택합니다.

   > ![image](https://user-images.githubusercontent.com/48195985/61778796-63f75300-ae3a-11e9-9fca-74625d31a24a.png)


   > 아래처럼 EC2 인스턴스가 선택되어 있는지 확인합니다.<p>
   > ![image](https://user-images.githubusercontent.com/48195985/61778827-783b5000-ae3a-11e9-8c1e-33835a4c662d.png)

**44. 작업 메뉴의 인스턴스 상태 --> 중지를 클릭합니다.**
   > ![image](https://user-images.githubusercontent.com/48195985/61778907-a91b8500-ae3a-11e9-8205-ad10d859ea4c.png)

**45. 인스턴스 중지 경고 창이 열리면 예,중지를 클릭합니다.**

   > 이 작업을 수행할 권한이 없다는 오류가 표시됩니다. 이는 정책이 변경 권한은 없고 정보확인만 가능함을 보여줍니다.<p>
   > ![image](https://user-images.githubusercontent.com/48195985/61778926-b89ace00-ae3a-11e9-84fb-3f07cdf92cc7.png)
   > ![image](https://user-images.githubusercontent.com/48195985/61778960-cd776180-ae3a-11e9-83a2-57f9d4b58a7e.png)

**46. 인스턴스 중지 창에 취소를 클릭합니다.**

**47. 서비스 메뉴에서 S3를 선택합니다.**

   > user-2 사용자가 Amazon S3에 대한 권한이 없기 때문에 Access Denied 되었습니다.<p>
   > ![image](https://user-images.githubusercontent.com/48195985/61780226-1c25fb00-ae3d-11e9-9786-5dbb06235a20.png)<p>
   > 이제 Amazon EC2 관리자로 채용된 user-3로 로그인 합니다.

**48. AWS Management Console에서 user-2 사용자 로그아웃합니다.**
   > * 화면 상단에 user-2를 클릭한 후 로그아웃을 클릭합니다.<p>
   > ![image](https://user-images.githubusercontent.com/48195985/61780394-64451d80-ae3d-11e9-921e-324539d7c33e.png)

**49. 웹 브라우저의 시크릿모드로 새창을 열고 IAM 사용자 로그인 링크를 붙여넣고 Enter를 입력합니다.**

**50. 다음 정보를 입력하여 로그인합니다.**
   > * 사용자 이름 : user-3
   > * 암호 : lab-password

**51. 서비스 메뉴에서 EC2를 클릭합니다.**

**52. 왼쪽 탐색창에서 인스턴스를 클릭합니다.**

   > EC2 관리자는  Amazon EC2 인스턴스를 중지할 수 있는 권한을 가지고 있어야 합니다.

**53. EC2 인스턴스를 선택하고 작업 메뉴의 인스턴스 상태 -> 중지를 클릭합니다.**
   > ![image](https://user-images.githubusercontent.com/48195985/61836669-3652ee00-aebc-11e9-832b-0f9f48db17e5.png)

**54. 인스턴스 중지 창에서 예.중지를 클릭합니다.**

   > 인스턴스가 중지 상태가되어 종료됩니다.

**55. 시크릿모드 웹 브라우저를 닫습니다.**

**56. AWS Management Console 창으로 돌아가 로그아웃 합니다.**

**57. ![image](https://user-images.githubusercontent.com/48195985/61836683-436fdd00-aebc-11e9-87dc-5f6f8cfa85ae.png)
을 클릭해서 LAB을 종료합니다.**
