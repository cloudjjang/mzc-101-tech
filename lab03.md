# 실습 : Introduction to Amazon Simple Storage Service (S3)

## LAB Overview:
  > Amazon S3(Amazon Simple Storage Service)는 업계 최고의 확장성, 데이터 가용성, 보안 및 성능을 제공하는 객체 스토리지 서비스입니다. 즉 모든 규모와 업종의 고객은 웹 사이트, 모바일 응용 프로그램, 백업 및 복원, 아카이브, 엔터프라이즈 응용 프로그램, IoT 장치 및 대형 데이터 분석과 같은 다양한 범위의 사용 사례에 대해 모든 규모의 데이터를 저장하고 보호할 수 있습니다. Amazon S3는 사용하기 쉬운 관리 기능을 제공하므로 데이터를 구성하고 특정 비즈니스, 조직 및 규정 준수 요구사항에 맞게 미세 조정된 액세스 제어를 구성할 수 있습니다. Amazon S3는 99.999999999% (11,9‘s)의 내구성을 위해 설계되었으며 전 세계 기업을 대상으로 수백만 건의 응용 프로그램 데이터를 저장합니다.

  > 이 실습에서는 AWS Management Console을 사용하여 Amazon S3 기본 사항을 설명합니다.

## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* Amazon S3에서 버킷 생성
* 객체를 버킷에 추가
* 객체 또는 버킷에 대한 액세스 권한 관리
* 버킷 정책 만들기
* 버킷 버전 관리 사용


## Start Lab:

**1. 상단 화면에  ![image](https://user-images.githubusercontent.com/48195985/61844251-b50a5400-aed9-11e9-92b0-cb5572b9790c.png)을 클릭하여 실습을 시작합니다.** 
  > 그러면 LAB 리소스 프로비저닝 프로세스가 시작됩니다. LAB 리소스를 구성하는데 소요되는 예상시간이 표시됩니다. 다음을 진행하기 전에 리소스 프로비저닝이 완료될 때까지 기다려야 합니다.


**2. ![image](https://user-images.githubusercontent.com/48195985/61844296-e4b95c00-aed9-11e9-8323-6790feb05b65.png)을 클릭하면 LAB을 위한 콘솔에 자동으로 로그인이 가능합니다.**
  
  > <span style="color:red">※ 다른 지시가 없는 경우 Region을 변경하지 마십시오.</span>

  > <span style="color:red">일반적인 로그인 오류:</span>

  > <span style="color:red">Error : Federated login credentials</span>

<br>

![image](https://user-images.githubusercontent.com/48195985/61844451-69a47580-aeda-11e9-9b48-3814d472eb57.png)\
**만약 위 메시지를 받는 경우 :**
* 웹 브라우저를 닫고 초기 LAB으로 이동합니다.
* 30초 정도 기다린 다음 ![image](https://user-images.githubusercontent.com/48195985/61844479-880a7100-aeda-11e9-90f8-b0f1fa289f6f.png)을 클릭합니다.


<span style="color:red">**Error : You must first log out**</span>\
![image](https://user-images.githubusercontent.com/48195985/61844528-b1c39800-aeda-11e9-976e-b7a24fee70f4.png)

위 메시지가 표시되면 다른 AWS 계정에 로그인 되어 있는 창을 로그아웃해야 합니다.

* 빨간 박스표시된 “Click here”를 클릭합니다.
* 기존 웹 브라우저가 닫히면 Qwiklabs 창으로 되돌아 갑니다.
* ![image](https://user-images.githubusercontent.com/48195985/61844479-880a7100-aeda-11e9-90f8-b0f1fa289f6f.png)을 다시 클릭합니다.


### 작업 1 : 버킷 만들기

Amazon S3의 모든 객체는 버킷에 저장됩니다. 이 작업에서는 버킷을 만들고 버킷 구성 옵션을 검사합니다.

**3. AWS Management Console에서 서비스 메뉴의 S3를 클릭합니다. 참고 : 서비스 메뉴 상단에서 S3을 입력하고 검색된 서비스를 클릭해도 됩니다.**


**4. S3 버킷을 생성하기 위해 ![image](https://user-images.githubusercontent.com/48195985/61844727-62ca3280-aedb-11e9-85e3-604d9e623db9.png)를 클릭합니다.**
> * 버킷 이름 : mybucket<span style="color:red">**NUMBER**</span>
> * <span style="color:red">**NUMBER**</span> 는 임의의 번호로 변경합니다.

> ※ 버킷 이름은 3 ~ 63자 사이 이여야하며, 숫자 또는 하이픈만 포함할 수 있습니다. 또한 계정 또는 지역에 관계없이 모든 Amazon S3에서 고유해야 하며 버킷을 만든 후에는 변경할 수 없습니다. 버킷이름을 입력하면 도움말 상자에 이름 지정 규칙의 위반 사항이 표시됩니다.

> * Region은 기본값을 사용합니다.
> 
> ※ 특정 Region을 선택하면 대기 시간을 최적화하거나 비용을 최소화하거나 규정요구 사항을 해결할 수 있습니다. 특정 영역에 저장된 객체는 명시적으로 다른 Region으로 전송하지 않는 한 해당 Region을 벗어나지 않습니다.

> * 기존의 버킷에서 복사 설정 옵션은 쉽게 다른 버킷과 같은 설정을 사용하여 버킷을 만들 수 있도록 하는데 사용할 수 있습니다. 이 실습에서는 이 옵션을 사용하지 않습니다.

**5. ![image](https://user-images.githubusercontent.com/48195985/61845096-e33d6300-aedc-11e9-905e-78b05299bf16.png)을 클릭합니다.**

**6. 옵션구성 화면에서 필요한 기능을 활성화 합니다. ![image](https://user-images.githubusercontent.com/48195985/61845105-eafd0780-aedc-11e9-9252-16573516e07b.png)를 클릭하면 자세한 내용을 확인할 수 있습니다.**

> 참고 : 옵션구성은 나중에 필요에 따라 재설정이 가능합니다. 이번 설정에서는 아무것도 선택하지 않고 진행합니다.

**7. ![image](https://user-images.githubusercontent.com/48195985/61845096-e33d6300-aedc-11e9-905e-78b05299bf16.png)을 클릭합니다.**

> 참고 : 기본적으로 새 S3 버킷 및 그 안에 포함된 객체는 공개적으로 액세스 할 수 없습니다. 퍼블릭 액세스를 특별히 활성화 해야하며 새 버킷을 만들 때 이 화면에서 액세스 할 수 있습니다. 

> 지금은 퍼블릭 액세스 차단 설정을 하지 않습니다.

**8. “권한 설정” 화면에서 기본값을 사용합니다.**


**9. ![image](https://user-images.githubusercontent.com/48195985/61845096-e33d6300-aedc-11e9-905e-78b05299bf16.png)을 클릭합니다.**


**10. “검토” 화면에서 버킷생성에 대한 설정을 검토하고 ![image](https://user-images.githubusercontent.com/48195985/61845192-3c0cfb80-aedd-11e9-8445-142a1ecd65fb.png)를 클릭합니다.**

<br>

### 작업 2 : 버킷에 개체 업로드하기

이제 버킷을 만들었으므로 객체를 저장할 준비가 되었습니다. 객체는 텍스트 파일, 사진, 비디오, zip 파일 등 모든 종류의 파일이 될 수 있습니다. Amazon S3에 객체를 추가할 때 객체 메타데이터를 포함하고 객체에 대한 액세스를 제어하는 권한을 설정할 수 있습니다.

**이 작업에서는 S3 버킷에 개체를 업로드 합니다.**

**11. “https://image.google.com” 사이트에서 ”sheep” 로 검색하고 적당한 이미지를 마우스 우측클릭 한 후 “이미지를 다른 이름으로 저장(v)”를 클릭하고 “sheep.jfif”로 저장합니다.**

**12. S3 콘솔에서 이전에 생성한 mybucket으로 시작하는 버킷을 클릭합니다.**

**13. ![image](https://user-images.githubusercontent.com/48195985/61845258-74143e80-aedd-11e9-8175-7bed79b3b209.png)를 클릭합니다.**

> 파일 업로드를 할 수 있는 업로드 마법사가 시작됩니다. 이 마법사를 사용하여 파일 선택기에서 파일을 선택하거나 S3 창으로 파일을 끌어서 업로드 합니다.

**14. ![image](https://user-images.githubusercontent.com/48195985/61845282-91e1a380-aedd-11e9-8725-2ee5349fd274.png)를 클릭합니다.**

**15. 이전에 다운로드한 sheep.jfif 파일을 찾아서 선택합니다.**

**16. ![image](https://user-images.githubusercontent.com/48195985/61845294-a32ab000-aedd-11e9-829d-021f9677efbf.png)를 클릭합니다.**

> 업로드 진행 상황은 화면 하단의 전송 패널에서 볼 수 있습니다. 매우 작은 파일이므로 전송이 표시되지 않을 수도 있습니다. 파일이 업로드되면 버킷에 표시됩니다.

> 참고 : 파일을 업로드 한후 몇 초 이내에 버킷에 파일이 표시되지 않으면 오른쪽 상단의 아이콘인 ![image](https://user-images.githubusercontent.com/48195985/61845321-bc336100-aedd-11e9-8fab-2249d02a9e51.png)
새로고침을 클릭합니다.

<br>

### 작업 3 : 객체 공개설정하기

이 작업에서는 객체를 공개적으로 액세스 할 수 있도록 버킷 및 객체에 대한 권한을 구성합니다.

**17. sheep.jfif 파일을 클릭합니다.**

> sheep.jfif 파일의 개요 페이지가 열립니다. 버킷 개요 페이지로 돌아가려면 화면 왼쪽 상단의 탐색에 링크가 표시됩니다.

**18. 창의 맨 아래에 표시된 “객체 URL” 링크를 복사합니다.**

링크는 다음과 유사합니다.\
https://mybucket9235.s3-us-west-2.amazonaws.com/sheep.jfif

**19. 새 브라우저 탭에서 링크를 주소 필드에 붙여넣기 한 다음 Enter를 누릅니다.**

> 액세스 거부 오류가 발생합니다. 이는 Amazon S3의 객체가 기본적으로 비공개이기 때문입니다.

**20. 이 브라우저를 열어두고 웹 브라우저의 S3 콘솔 탭으로 돌아갑니다.**

**21. sheep.jfif 개요 페이지에서 권한 탭을 클릭합니다.**

**22. “퍼블릭 액세스” 섹션 아래 ![image](https://user-images.githubusercontent.com/48195985/61845399-13d1cc80-aede-11e9-9746-66a011eca13a.png)을 선택합니다. 오른쪽 사이드에 새로운 설정 화면이 나타납니다.**

**23. ![image](https://user-images.githubusercontent.com/48195985/61845410-20eebb80-aede-11e9-9562-babb6d5a421f.png)를 체크하면 개체가 퍼블릭 액세스 권한이 생긴다는 경고 메시지가 발생합니다.** 

**24. ![image](https://user-images.githubusercontent.com/48195985/61845423-2b10ba00-aede-11e9-828c-f2a05cc02c31.png)을 클릭합니다.**

> 버킷은 퍼블릭 액세스를 허용하지 않도록 구성되어 있기 때문에 오류 메시지가 화면상단에 표시됩니다. 버킷 설정은 개별 객체에 적용되는 권한을 무시합니다.

**25. 화면의 왼쪽 상단에서 이전에 만든 버킷 이름의 링크를 클릭합니다.**

**26. “권한” 탭을 클릭합니다.  ![image](https://user-images.githubusercontent.com/48195985/61845450-48de1f00-aede-11e9-8e1b-673f186d41ce.png)버튼이 강조 표시됩니다.**

**27. 우측에 있는 ![image](https://user-images.githubusercontent.com/48195985/61845456-52678700-aede-11e9-8004-5e7da355d8c9.png)을 클릭하여 설정을 변경합니다.**

**28. 모든 퍼블릭 액세스 차단의 체크박스를 해제 합니다.**

> ※ 모든 개별 옵션은 선택 취소된 상태로 유지됩니다. 모든 퍼블릭 액세스 차단을 해제할 때는 보안 목적에 맞는 개별 옵션을 선택해야 합니다. 해당 실습에서는 나눙에 ACL과 버킷정책을 사용하므로 이 예제에서는 모도 해제된 상태로 유지합니다. 프로덕션 환경에서는 최소한의 허용설정을 사용하는 것이 좋습니다.

**29. ![image](https://user-images.githubusercontent.com/48195985/61845423-2b10ba00-aede-11e9-828c-f2a05cc02c31.png)을 클릭합니다.**

**30. 변경 사항을 확인하라는 대화상자가 열립니다. 확인 필드에 “확인”을 입력하고 ![image](https://user-images.githubusercontent.com/48195985/61845493-6f9c5580-aede-11e9-8d42-ea7cc8b8db9e.png)을 클릭합니다.**

> “퍼블릭 액세스 설정을 성공적으로 업데이트함” 메시지가 표시됩니다.

**31. “개요” 탭을 클릭합니다.**

**32. sheep.jfif 파일을 클릭합니다.**

**33. sheep.jfif 개요 페이지에서 “권한”탭을 클릭합니다.**

**34. “퍼블릭 액세스” 섹션 아래 ![image](https://user-images.githubusercontent.com/48195985/61845399-13d1cc80-aede-11e9-9746-66a011eca13a.png)을 선택합니다. 오른쪽 사이드에 새로운 설정 화면이 나타납니다.**

**35. ![image](https://user-images.githubusercontent.com/48195985/61845410-20eebb80-aede-11e9-9562-babb6d5a421f.png)를 체크하면 개체가 퍼블릭 액세스 권한이 생긴다는 경고 메시지가 발생합니다.**

**36. ![image](https://user-images.githubusercontent.com/48195985/61845423-2b10ba00-aede-11e9-828c-f2a05cc02c31.png)을 클릭합니다.**

**37. 액세스 거부가 표시되어 있는 웹 브라우저로 돌아가 새로고침 표시를 누릅니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61845586-e89bad00-aede-11e9-9b5a-48de8b64132b.png)\
> 객체를 퍼블릭 액세스할 수 있도록 설정하였으므로 그림이 표시됩니다.

**38. 그림이 표시된 웹 브라우저를 닫고 Amazon S3 콘솔로 돌아갑니다.**

> 이 예제에서는 특정 객체에 대한 읽기 액세스 권한을 부여했습니다. 전체 버킷에 액세스 권한을 부여하려면 버킷 정책을 사용합니다.

<br>

### 작업 4 : 버킷 정책 만들기

버킷 정책은 S3 버킷과 관련된 권한 집합입니다. 전체 버킷 또는 버킷 내의 특정 디렉토리에 대한 액세스를 제어하는데 사용할 수 있습니다. 이 작업에서는 새 파일을 버킷에 업로드하고 공개적으로 액세스 할 수 없는지 확인합니다. 그런 다음 AWS 정책 생성기를 사용하여 버킷 내의 모든 객체에 대한 공용 읽기 액세스를 가능하게 하는 버킷 정책을 만듭니다.

**39. 노트패드를 열어 다음 내용을 입력하고 sample-file.txt 로 저장합니다.** 

``` python
“ This sample text file is used to illustrate the use of versioning in an Amazon S3 bucket. ”
```

**40. S3 콘솔 탭에서 왼쪽 상단의 버킷 이름을 클릭합니다.**

**41. ![image](https://user-images.githubusercontent.com/48195985/61845707-5fd14100-aedf-11e9-8fb3-da249b17e134.png)를 클릭하여 이전에 업로드 했던 방법대로 sample-file.txt 파일을 찾아 업로드 합니다.**

**42. 업로드된 sample-file.txt 파일을 클릭하면 개요 페이지가 열립니다.**

**43. 창의 맨 아래 “객체 URL” 링크를 복사합니다.**

**44. 새 브라우저 탭에서 링크를 주소 필드에 붙여넣은 다음 Enter를 누릅니다.**

> 다시 액세스 거부 에러 메시지가 표시됩니다. 각 객체에 개별적으로 권한을 지정할 필요없이 버킷의 모든 객체에 대한 액세스를 허용하도록 버킷 정책을 구성해야 합니다.

**45. 이 브라우저의 탭을 열어두고 S3 콘솔로 돌아갑니다.**

**46. 창 왼쪽 상단에서 버킷 이름을 클릭합니다.**

> 버킷에 있는 개체의 목록을 볼 수 있습니다. 그렇치 않은 경우 업로드 한 객체 목록을 볼 수 있도록 버킷으로 다시 이동하십시오.

**47. 권한 탭을 클릭합니다.**

**48. 권한 탭에서 ![image](https://user-images.githubusercontent.com/48195985/61845761-8d1def00-aedf-11e9-9656-654aee180a83.png)
을 클릭합니다.**

> 빈 버킷 정책 편집기가 표시됩니다. 버킷 정책은 수동으로 생성하거나 AWS 정책 생성기를 이용해서 생성할 수 있습니다.

> 정책을 만들기 전에 버킷의 ARN(Amazon Resource Name)을 복사해야 합니다.

> ※ ARN은 모든 AWS에서 AWS 리소스를 고유하게 식별합니다. ARN으 각 섹션은 “:”로 구분되며 지정된 자원에 대한 경로의 특정 부분을 나타냅니다. 섹션은 서비스에 따라 약간 다를 수 있지만 일반적으로 다음 형식을 따릅니다.

> arn:<span style="color:red">_partition:service:region:account-id:resource_</span>

> Amazon S3는 ARN에서 Region 또는 account-id의 매개변수를 필요로 하지 않으므로 해당 섹션은 공백으로 남깁니다. 그러나 섹션을 구분하는 “:”은 여전히 사용되어야 하므로  **arn:aws:s3:::mybucket9235** 와 같은 형태로 사용됩니다.

**49. 버킷의 ARN을 클립보드에 복사합니다. 정책편집기 위쪽에 명시되어 있습니다.**

**50. 페이지 하단의 정책생성기를 클릭합니다.**

> 새로운 웹 브라우저에 AWS 정책생성기 페이지가 열립니다.

> ※ AWS 정책은 JSON 형식을 사용하며 AWS서비스에 대한 세부적인 권한을 구성하는데 사용됩니다. AWS 정책생성기를 사용하면 JSON으로 정책을 수동으로 작성할 수도 있지만 친숙한 웹 인터페이스에서 몇 번의 클릭만으로 정책을 작성할 수 있습니다.

**51. AWS 정책생성기 창에서 다음을 구성합니다.**

* Select Type of Policy : S3 Bucket Policy
* Effct : Allow
* Principal : *

> **위 설정은 누구나 정책에서 조치를 수행 할 수 있음을 의미한다.**

* AWS Service : Amazon S3
* Actions : ![image](https://user-images.githubusercontent.com/48195985/61846372-d8d19800-aee1-11e9-94b6-f128141344a0.png)

> **getObject 액션은 Amazon S3에서 검색 할 객체에 대한 권한을 부여합니다.**

* Amazon Resource Name(ARN) : 이전에 복사한 ARN을 붙여 넣습니다.
* ARN 끝부분에 /* 를 추가합니다.

> ![image](https://user-images.githubusercontent.com/48195985/61846399-e8e97780-aee1-11e9-8009-62db21142408.png)

> Amazon Resource Name(ARN)은 AWS내의 리소스를 참조하는 표준 방법입니다. 이 경우 ARN이 S3 버킷을 참조합니다. 버킷 이름 끝에 /*를 추가하면 정책이 버킷내의 모든 객체에 적용됩니다.

**52. ![image](https://user-images.githubusercontent.com/48195985/61846457-2817c880-aee2-11e9-9e5a-05d54c7cec87.png)를 클릭합니다. 구성한 명령문의 세부 사항이 버튼 아래의 테이블에 추가됩니다. 하나의 정책에 여러 명령문을 추가할 수 있습니다.**

**53. ![image](https://user-images.githubusercontent.com/48195985/61846465-2f3ed680-aee2-11e9-9a9d-833d8c5f64f2.png)을 클릭합니다.**

> JSON 형식으로 생성된 정책을 보여주는 새창이 표시됩니다. 다음과 유사합니다.


``` json
{
  "Id": "Policy1563157677183",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1563157667217",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::mybucket-9235/*",
      "Principal": "*"
    }
  ]
}
```
> ※ Resource 줄에 ARN의 버킷 이름 뒤에 /* 를 확인합니다.

**54. 정책을 클립보드로 복사합니다. (선택해서 Ctrl + c 로 복사하기)**

**55. 웹 브라우저 탭을 닫고 버킷 정책 편집기 탭으로 돌아갑니다.**

**56. 버킷 정책을 버킷 정책 편집기에 붙여넣습니다.**

**57. ![image](https://user-images.githubusercontent.com/48195985/61846542-66ad8300-aee2-11e9-8034-a5352a6f7f44.png)을 클릭합니다.**

> 버킷에 버킷정책을 적용했습니다. 이 정책을 사용하면 버킷의 모든 개체에 퍼블릭 액세스가 가능해 집니다.

> ※ 이 버킷에 퍼블릭 액세스 권한이 있는 화면 상단의 경고 메시지 배너를 확인합니다. 

![image](https://user-images.githubusercontent.com/48195985/61846568-8644ab80-aee2-11e9-868c-54e213380624.png)

**58. 액세스 거부되어 내용을 확인할 수 없던 웹 브라우저 탭으로 돌아가 새로고침 합니다.**

이제 업로드한 파일에 포함된 텍스트가 페이지에 표시됩니다. 이는 버킷정책이 전체적으로 버킷에 적용되었기 때문에 개별 개체에 개별적으로 권한을 부여할 필요가 없습니다.

**59. 이 웹 브라우저 탭을 다음 작업을 위해 열어두고 S3 콘솔 탭으로 돌아갑니다.**

<br>


### 작업 5 : 버전관리 탐색하기

버전관리는 객체의 여러 변경내용을 동일한 버킷에 유지하는 수단입니다. 버전 관리를 사용하여 Amazon S3 버킷에 저장된 모든 객체의 모든 버전을 유지, 검색 및 복원 할 수 있습니다. 버전 관리를 사용하념 의도하지 않은 사용자 작업 및 응용 프로그램 오류를 쉽게 복구할 수 있습니다.

이 작업에서는 버킷의 버전관리를 활성화 한 다음 이전 작업에서 사용한 텍스트 파일의 수정된 버전을 업로드 합니다.

**60. 이전 작업에서 생성한 버킷(mybucket으로 시작하는 버킷)을 클릭합니다.**

**61. 속성 탭을 클릭합니다.**

**62. 버전관리를 클릭합니다.**

**63. ![image](https://user-images.githubusercontent.com/48195985/61846611-b4c28680-aee2-11e9-8674-fdf6bffe0be1.png)을 체크한 후 ![image](https://user-images.githubusercontent.com/48195985/61846542-66ad8300-aee2-11e9-8034-a5352a6f7f44.png)을 클릭합니다. (이미 선택된 경우 저장한 하면 됩니다.)**

> ※ 버전 관리는 전체 버킷 및 버킷 내의 모든 객체에 대해 활성화 됩니다. 개별 개체에 대해서는 활성화 할 수 없습니다.

> ※ 버전 관리를 사용할 때 비용 고려 사항도 있습니다. 자세한 사항은 S3 문서의 “[Amazon S3 버전 관리 비용 고려사항](https://aws.amazon.com/ko/s3/faqs/)”을 참고하십시오.

**64. 컴퓨터에 저장되어 있는 sample-file.txt 파일을 열어 다음 내용을 추가하고 저장합니다.**
    
        “This is version 2 of the file.”

> ※ 이전 파일에 새로운 내용을 추가해서 S3 버킷에 업데이트를 합니다.

**65. S3 콘솔에서 개요 탭을 클릭합니다.**

  > ![image](https://user-images.githubusercontent.com/48195985/61846972-0b7c9000-aee4-11e9-87b3-b9d36899e4f5.png)버튼이 표시됩니다.


**66. ![image](https://user-images.githubusercontent.com/48195985/61847007-1a634280-aee4-11e9-8acc-108f1fb47d81.png)를 클릭하여 이전에 업로드 했던 방법대로 sample-file.txt 파일을 찾아 업로드 합니다.**

**67. sample-file.txt 파일의 내용이 출력되어 있는 웹 브라우저로 이동합니다.**

> 만약 sample-file.txt 파일을 열어놓은 파일이 없는 경수 sample-file.txt 파일의 객체 URL 정보를 복사해서 새로운 웹페이지 열면 됩니다.

**68. 페이지의 내용을 “새로고침” 합니다. (마우스 우 클릭하고 “새로고침(R)” 선택)**

> 새로 추가된 텍스트가 출력됩니다.

> ※ Amazon S3는 버전이 별도로 지정되지 않는 경우 항상 최신버전의 객체를 반환합니다.

> S3 콘솔에서 사용가능한 버전 목록을 얻을 수도 있습니다.

**69. 텍스트가 출력되어 있는 웹 브라우저 탭을 닫습니다.**

**70. S3 콘솔에서 sample-file.txt 파일 이름을 클릭합니다. sample-file.txt 개요 페이지가 열립니다.**


71. 개요 페이지의 파일이름 옆의 ![image](https://user-images.githubusercontent.com/48195985/61847093-5696a300-aee4-11e9-8d31-acf89a6f8ea0.png)
을 클릭합니다.  (최신버전) 표시가 없는 아래 첫 번째 파일을 클릭합니다.

> ![image](https://user-images.githubusercontent.com/48195985/61847101-60200b00-aee4-11e9-967a-70af5fe6df34.png)

72. ![image](https://user-images.githubusercontent.com/48195985/61847112-6ca46380-aee4-11e9-9539-fd92e4dc3e97.png)를 클릭합니다.

> 이제 S3 콘솔의 열기 버튼을 사용하여 파일의 첫 번째 버전을 볼 수 있습니다.

> ※ 그러나 객체 URL 링크를 사용하여 이전 sample-file.txt 파일에 액세스 하려고 하면 액세스 거부 메시지가 표시됩니다. 이는 이전 작업에서 만든 버킷 정책이 객체의 최신 버전에 액세스 권한만 허용하기 때문입니다. 이전 버전의 객체에 액세스 하려면 “s3:GetObjectVersion” 권한을 버킷 정책에 추가해야 합니다. 다음은 객체 URL 링크를 통해 액세스 가능하도록 권한을 설정하는 버킷정책 예제입니다.

```json
{
  "Id": "Policy1557511288767",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1557511286634",
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::mybucket45647467/*",
      "Principal": "*"
    }
  ]
}
```

**73. 버킷 개요 탭으로 돌아가려면 왼쪽 상단의 버킷 이름 링크를 클릭합니다.**

**74. 객체 목록 위에 있는  ![image](https://user-images.githubusercontent.com/48195985/61847131-8e9de600-aee4-11e9-8080-6190cd1c0026.png)버튼의 “표시”를 클릭합니다.**

> 보기가 변경되어 각 파일의 버전과 최신버전을 표시합니다. sheep.jfif 파일의 경우 버전ID가 null 로 표시되어 있습니다. 이는 버전관리가 활성화 되기전에 객체가 업로드 되었기 때문입니다.

> 또한 버전 이름 링크를 클릭하면 콘솔에서 해당 버전의 객체로 바로 이동할 수 있습니다.

**75.  ![image](https://user-images.githubusercontent.com/48195985/61846972-0b7c9000-aee4-11e9-87b3-b9d36899e4f5.png)버튼을 클릭하여 기본 객체 보기로 돌아갑니다.**

**76. sample-file.txt 파일의 왼쪽에 있는 선택란을 선택합니다.**

**77. ![image](https://user-images.githubusercontent.com/48195985/61847215-d02e9100-aee4-11e9-8da7-1fd0b95aeb9a.png)을 클릭합니다. 출력된 메뉴중에 “삭제”를 선택합니다.**

**78. 객체 삭제 창이 열리면  ![image](https://user-images.githubusercontent.com/48195985/61847234-e0df0700-aee4-11e9-9e11-6513af578ef6.png)버튼을 클릭합니다.**

> sample-file.txt 객체는 더 이상 버킷에 표시되지 않습니다. 그러나 실수로 객체를 삭제하였다면 버전관리를 통해 객체를 복구할 수 있습니다.

79. 버튼을 클릭합니다.

sample-file.txt 객체가 다시 표시되지만 가장 최근 버전은 (삭제 머커)가 표시되어 있습니다. 두 이전 버전도 나열되어 있습니다. 버킷에서 버전관리를 사용하도록 설정하면 객체가 즉시 삭제되지 않습니다. 대신 Amazon S3는 현재 객체에 삭제마커를 삽입합니다. 이전 버전의 객체는 제거되지 않습니다.

**80. sample-file.txt 객체의 버전 왼쪽에 있는 (삭제 마커) 확인란을 선택하십시오.**

**81. ![image](https://user-images.githubusercontent.com/48195985/61847215-d02e9100-aee4-11e9-8da7-1fd0b95aeb9a.png)을 클릭합니다. 출력된 메뉴중에 “삭제”를 선택합니다.**

**82. 객체 삭제 창이 열리면  ![image](https://user-images.githubusercontent.com/48195985/61847234-e0df0700-aee4-11e9-9e11-6513af578ef6.png)버튼을 클릭합니다.**

**83. ![image](https://user-images.githubusercontent.com/48195985/61847131-8e9de600-aee4-11e9-8080-6190cd1c0026.png)버튼을 클릭합니다.**

> sample-file.txt 객체가 버킷으로 복원되었습니다. 삭제마커를 제거하면 객체를 이전 상태로 효과적으로 복원할 수 있습니다.

> 특정 버전의 객체를 삭제할 수도 있습니다.

**84. 객체 목록 위에 있는 ![image](https://user-images.githubusercontent.com/48195985/61847131-8e9de600-aee4-11e9-8080-6190cd1c0026.png)버튼을 클릭합니다.**

> sample-file.txt 객체의 두가지 버전을 확인할 수 있어야 합니다.

**85. sample-file.txt 객체의 최신버전 왼쪽에 있는 선택란을 체크합니다.**

**86. ![image](https://user-images.githubusercontent.com/48195985/61847215-d02e9100-aee4-11e9-8da7-1fd0b95aeb9a.png)을 클릭합니다. 출력된 메뉴중에 “삭제”를 선택합니다.**

**87. 객체 삭제 창이 열리면  ![image](https://user-images.githubusercontent.com/48195985/61847234-e0df0700-aee4-11e9-9e11-6513af578ef6.png)버튼을 클릭합니다.**

> 현재 sample-file.txt 파일의 버전은 하나뿐입니다. 특정 버전의 개체를 삭제할 때는 (삭제 마커)가 생성되지 않고 객체가 영구적으로 삭제됩니다. 

**88.  ![image](https://user-images.githubusercontent.com/48195985/61846972-0b7c9000-aee4-11e9-87b3-b9d36899e4f5.png)버튼을 클릭합니다.**

**89. sample-file.txt 파일 이름을 클릭합니다. sample-file.txt 파일의 개요 페이지가 열립니다.**

**90. 창의 맨 아래에 표시된 “객체 URL” 링크를 복사합니다.**

**91. 새 브라우저 탭에서 복사한 객체 URL 링크 주소를 붙여넣기 한 다음 Enter를 누릅니다.**

> sample-file.txt 객체의 원래 버전의 텍스트 내용이 표시됩니다.
