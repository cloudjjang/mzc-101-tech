![](https://s3-us-west-2.amazonaws.com/us-west-2-aws-training/awsu-spl/sts-sign-in-images/media/aws-logo.png)

# Amazon Polly를 사용하여 서버리스 텍스트 음성 변환 애플리케이션 구축

<br>
<br>

## 개요

일반적으로 음성 합성은 쉽지 않습니다. 응용 프로그램이 문장의 각 문자를 읽을 때 출력이 의미가 있다고 가정 할 수 없습니다. *TTS (text-to-speech)* 응용 프로그램의 몇 가지 일반적인 문제는 다음과 같습니다.

- 같은 방식으로 쓰여졌지만 다르게 발음되는 단어 : I **live** in Las Vegas compared to This presentation broadcasts **live** from Las Vegas.
- 텍스트 정규화 : 명확한 약어, 머리글자 및 단위 : **St..** 은 **Street** 또는 **Saint**로 확장될 수 있다.
- 단어와 문맥에 따라 다른 단어의 유사한 부분이 다르게 발음될 수 있음. (**tough**, **through**, **though**)
- 외래어(deja vu), 고유명사(Francois Hollande) 및 속어(ASAP, LOL).

**Amazon Polly** 는 이러한 과제를 극복하는 음성 합성 기능을 제공하므로 해석 과제를 해결하는 대신 텍스트 음성 변환을 사용하는 애플리케이션을 구축하는 데 집중할 수 있습니다.

**Amazon Polly**는 텍스트를 실제와 같은 음성으로 변환합니다. 자연스럽게 대화하는 응용 프로그램을 만들어 완전히 새로운 범주의 음성 지원 제품을 만들 수 있습니다. 

**Amazon Polly**는 고급 딥 러닝 기술을 사용하여 사람의 목소리처럼 들리는 음성을 합성하는 Amazon AI 서비스입니다. 현재 20 개 이상의 언어로 된 수십 가지의 생생한 음성이 포함되어 있으므로 여러 국가에서 이상적인 음성을 선택하고 음성 기반 응용 프로그램을 구축 할 수 있습니다.

또한 **Amazon Polly**는 실시간 대화식 대화를 지원하는 데 필요한 일관된 빠른 응답 시간을 제공합니다. 오프라인 재생 또는 재배포를 위해 Polly의 오디오 파일을 캐시하고 저장할 수 있습니다. 즉, 변환하고 저장하는 것은 사용자의 것입니다. 음성 사용에 대한 추가 텍스트 음성 변환 비용은 없습니다. Polly도 사용하기 쉽습니다. 음성으로 변환하려는 텍스트를 Amazon Polly API로 보내기 만 하면 됩니다. Amazon Polly는 오디오 스트림을 즉시 애플리케이션으로 반환하여 애플리케이션에서 직접 재생하거나 MP3와 같은 표준 오디오 파일 형식으로 저장할 수 있습니다.

이 실습에서는 **Amazon Polly**를 사용하여 텍스트를 음성으로 변환하는 기본 서버리스 애플리케이션을 생성합니다. 이 응용 프로그램에는 여러 언어로 된 텍스트를 받아 들인 다음 웹 브라우저에서 재생할 수 있는 오디오 파일로 변환하는 간단한 사용자 인터페이스가 있습니다. 이 실습에서는 블로그 게시물을 사용하지만 모든 유형의 텍스트를 사용할 수 있습니다. 예를 들어, 식사를 준비하는 동안 레시피를 읽거나 자전거를 타거나 운전하는 동안 뉴스 기사 나 책을 읽는 데 응용 프로그램을 사용할 수 있습니다.

<br>
<br>

## 응용 프로그램 아키텍처

서버리스 애플리케이션을 구축 할 수 있습니다. 관리형 서비스를 사용하므로 프로비저닝, 패치 적용, 스케일링 등 서버 작업을 할 필요가 없습니다. AWS 클라우드는 자동으로 이를 처리하므로 애플리케이션에 집중할 수 있습니다.

애플리케이션은 두 가지 방법을 제공합니다. 하나는 MP3 파일로 변환해야 하는 새 게시물에 대한 정보를 전송하는 방법과 Amazon S3 버킷에 저장된 MP3 파일에 대한 링크를 포함하여 게시물에 대한 정보를 검색하는 방법입니다. 두 방법 모두 Amazon API Gateway를 통해 RESTful 웹 서비스로 노출됩니다.

실습의 최종 아키텍처는 다음과 같습니다.

![Architecture](https://s3.ap-northeast-2.amazonaws.com/mzclab.com/qlabsimages/MSA-Policy-Topology-1.jpg)


** 애플리케이션이 새 게시물에 대한 정보를 전달할 때 :

 <span style="background-color:#0174DF; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">1</span> 정보는 Amazon API Gateway에 의해 노출 된 RESTful 웹 서비스에 의해 수신됩니다. 이 웹 서비스는 Amazon Simple Storage Service (Amazon S3)에서 호스팅되는 정적 웹 페이지에 의해 호출됩니다.

 <span style="background-color:#0174DF; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">2</span> Amazon API Gateway는 MP3 파일 생성 프로세스를 초기화 하는 AWS Lambda 함수인 New Post를 트리거 합니다.

 <span style="background-color:#0174DF; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">3</span> Lambda 함수는 게시물에 대한 정보를 Amazon DynamoDB 테이블에 삽입 합니다. 여기서 모든 게시물에 대한 정보가 저장됩니다

 <span style="background-color:#0174DF; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">4</span> 전체 프로세스를 비동기식으로 실행하려면 Amazon SNS( Amazon Simple Notification Service)를 사용하여 새 게시물에 대한 정보를 수신하고 오디오 변환을 시작하는 프로세스를 분리합니다.

 <span style="background-color:#0174DF; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">5</span> 다른 Lambda 기능인 *Convert to Audio*는 SNS 주제에 가입되어 새 메시지가 나타날 때마다 트리거됩니다. (새 게시물을 오디오 파일로 변환합니다)

 <span style="background-color:#0174DF; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">6</span> *오디오로 변환* 하는 람다 함수가 Amazon Polly를 사용해서 특정 언어 (텍스트의 언어와 같은)로 텍스트를 오디오 파일로 변환한다.

 <span style="background-color:#0174DF; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">7</span> 변환된 MP3 파일은 전용 S3 버킷에 저장됩니다.

 <span style="background-color:#0174DF; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">8</span> 게시물에 대한 정보가 DynamoDB 테이블에서 업데이트 됩니다. S3 버킷에 저장된 오디오 파일의 URL은 이전에 저장된 데이터와 함께 저장됩니다.

<br>

** 애플리케이션이 새 게시물에 대한 정보를 검색할 때 :

 <span style="background-color:#8A0808; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">1</span> RESTful 웹 서비스는 **Amazon API Gateway** 를 사용하여 배포됩니다 . Amazon API Gateway는 게시물에 대한 정보를 검색하는 방법을 제공합니다. 이러한 방법에는 게시물의 텍스트와 MP3 파일이 저장된 S3 버킷에 대한 링크가 포함됩니다. 웹 서비스는 Amazon S3에서 호스팅 되는 정적 웹 페이지에 의해 호출됩니다.

 <span style="background-color:#8A0808; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">2</span> Amazon API Gateway는 게시물 데이터 검색을 위한 로직을 배포한 _**Get Post**_ Lambda 함수를 호출합니다 .

 <span style="background-color:#8A0808; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">3</span> _**Get Post**_ Lambda 함수는 DynamoDB 테이블에서 게시물(Amazon S3 참조 포함)에 대한 정보를 검색하고 정보를 반환합니다.


### Topics covered
이 실습을 마치면 다음을 할 수 있습니다.
- 데이터를 저장할 Amazon DynamoDB 생성
- Amazon API Gateway RESTful API 생성
- API Gateway에 의해 트리거 된 AWS Lambda 함수 생성
- Amazon Simple Notification Service(SNS)와 AWS Lambda 기능 연결
- Amazon Polly를 사용하여 다양한 언어의 텍스트를 음성으로 변환

<br>
<br>

**소요 시간**

이 실습은 완료하는 데 약 **60분** 이 소요됩니다.

___
## AWS Management Console에 액세스

1. <span style="background-color:#34A853; font-weight:bold; font-size:90%; color:white; border-radius:2px; padding-left:10px; padding-right:10px; padding-top:3px; padding-bottom:3px">Start Lab(실습 시작)</span>을 클릭하여 실습을 시작합니다.

그러면 실습 리소스 프로비저닝 프로세스가 시작됩니다. 실습 리소스를 프로비저닝하는데 걸리는 예상 시간이 표시됩니다. 계속하기 전에 리소스가 프로비저닝 될 때까지 기다려야합니다.

2. <span style="background-color:white; font-family:Google Sans; font-weight:bold; font-size:90%; color:#1a73e8; border-color:#dadce0; border-radius:4px; border-width:2px; border-style:solid; outline-color:#ffffff; padding-top:5px; padding-bottom:5px; padding-left:10px; padding-right:10px">Open Console(콘솔 열기)</span> 클릭해서 실습을 엽니다.

그러면 AWS Management Console에 자동으로 로그인 됩니다.

<i class="fas fa-exclamation-triangle"></i> 이 실습 도중에 리전을 변경하지 마십시오.

___

## 작업 1: DynamoDB 테이블 생성하기

애플리케이션은 MP3 파일의 텍스트 및 URL을 포함하여 블로그 게시물에 대한 정보를 Amazon DynamoDB에 저장합니다. 게시물 테이블 작성을 시작합니다. Primary key(id)는 새 레코드(posts)가 데이터베이스에 삽입 될 때 _New Post Lambda_ 함수가 생성하는 문자열 입니다.

3. **AWS Management Console**의 <span style="background-color:#232f3e; font-weight:bold; font-size:90%; color:white; position:relative; top:-1px; padding-top:3px; padding-bottom:3px; padding-left:10px; padding-right:10px;">Services(서비스)<i class="fas fa-angle-down"></i></span> 메뉴에서 **DynamoDB**를 클릭합니다.
<br>

4. 다음을 사용하여 새 DynamoDB 테이블을 생성합니다.
- Table name: ```posts```
- Primary key: ```id```(String)
- <i class="far fa-check-square"></i> Use default settings</li>

이제 테이블의 전체 구조를 정의 할 필요가 없습니다. 대신 응용프로그램이 다음과 같은 레코드로 채우게 됩니다.

![DynamoDB-Table](https://s3.ap-northeast-2.amazonaws.com/mzclab.com/qlabsimages/polly-img-dynamodb-1.jpg)


응용프로그램이 다음을 저장합니다 :
- **id** : 게시물의 id
- **status** : MP3 파일이 이미 작성되었는지에 따라 UPDATA 또는 PROCESSING
- **text** : 오디오 파일이 생성되는 게시물의 텍스트
- **url** : 오디오 파일이 저장되는 S3 버킷의 링크
- **voice** : 오디오 파일을 생성하는데 사용된 Amazon Polly 음성

---

## 작업2 : Amazon S3 Bucket 생성하기
애플리케이션에서 생성한 모든 오디오 파일을 저장하려면 Amazon S3 버킷을 생성해야합니다. _audioposts-123_ 과 같은 고유한 이름으로 버킷을 생성합니다.

5. **AWS Management Console**의 <span style="background-color:#232f3e; font-weight:bold; font-size:90%; color:white; position:relative; top:-1px; padding-top:3px; padding-bottom:3px; padding-left:10px; padding-right:10px;">Services(서비스)<i class="fas fa-angle-down"></i></span> 메뉴에서 **S3**를 클릭합니다.

6. <span style="background-color:#2E64FE; font-weight:bold; font-size:90%; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:10px; padding-right:10px;white-space: nowrap"><i class="far fa-plus"> Create Bucket(버킷 만들기)</span>를 클릭한 후 다음을 구성합니다.
    - 버킷이름 : ```autdiposts-NUMBER```
    - 버킷이름의 NUMBER의 숫자는 임의의 수로 입력 
    - 버킷이름을 텍스트편집기로 복사합니다. 나중에 사용합니다.
    - Region(지역) : Region(지역)을 변경하지 마십시오.
    - <span style="background-color:#FAFAFA; font-weight; font-size:80%; color:#013ADF; position:relative; top:-1px; border-radius:2px; border-width:1px; border-style:solid; border-color:#444; padding-top:3px; padding-bottom:3px; padding-left:10px; padding-right:10px;white-space: nowrap">Create(생성)</span>를 클릭 하십시오.


애플리케이션은이 버킷을 사용하여 Amazon Polly에서 생성 한 오디오 파일을 저장합니다.

7. 방금 생성한 버킷을 클릭하십시오.
8. **Permissions(권한)** 탭을 클릭합니다.
9.  **Block public access(공개 액세스 차단)** 내용의 박스 우측에 있는 <span style="background-color:#FAFAFA; font-weight; font-size:80%; color:#013ADF; position:relative; top:-1px; border-radius:2px; border-width:1px; border-style:solid; border-color:#444; padding-top:3px; padding-bottom:3px; padding-left:10px; padding-right:10px;white-space: nowrap">Edit(편집)</span>을 클릭한 후 다음을 구성합니다.
      - 선택해제 : <i class="far fa-square"></i> Block all public access(모든 퍼블릭 액세스 차단)을 체크 해제 합니다.
      - <span style="background-color:#0080FF; font-weight; font-size:80%; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:5px; padding-right:5px;white-space: nowrap">Save</span>를 클릭합니다.

10. Edit Block public access(bucket setting) 편집창에서 :
      - confirm 을 입력하고 <span style="background-color:#0080FF; font-weight; font-size:80%; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:5px; padding-right:5px;white-space: nowrap">Confirm</span>를 클릭합니다.


___
## 실습 완료

<i class="icon-flag-checkered"></i> 축하합니다! 실습을 마치셨습니다.

## 실습 종료

다음 단계를 따라 콘솔을 닫고, 실습을 종료하고, 경험을 평가하십시오.

77. [77]AWS Management Console로 돌아갑니다.

78. [78]탐색 바에서 **&lt;yourusername&gt;@&lt;AccountNumber&gt;** 를 클릭하고 **Sign Out(로그아웃)** 을 클릭합니다.

79. [79]<span style="background-color:#D93025; font-family:Google Sans; font-weight:bold; font-size:90%; color:white; border-color:#D93025; border-radius:4px; border-width:2px; border-style:solid; outline-color:#ffffff; padding-top:5px; padding-bottom:5px; padding-left:10px; padding-right:10px">End Lab(실습 종료)</span>을 클릭합니다.

80. [80]<span style="background-color:#DEDEDE; font-family:Google Sans; font-weight:bold; font-size:90%; color:#444; position:relative; top:-1px; border-width:1px; border-style:solid; border-color:#444; padding-top:3px; padding-bottom:3px; padding-left:10px; padding-right:10px">OK(확인)</span>를 클릭합니다.

81. [81]\(선택사항\):

* 해당하는 별 개수를 선택합니다.<i class="far fa-star"></i>
* 의견 입력
* **Submit(제출)** 을 클릭합니다.

 * 별 1개 = 매우 불만족
 * 별 2개 = 불만족
 * 별 3개 = 보통
 * 별 4개 = 만족
 * 별 5개 = 매우 만족

피드백을 제공하지 않으려면 대화 상자를 닫으면 됩니다.
