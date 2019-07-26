# 실습 : Introduction to AWS Lambda

## LAB Overview:
이 실습실에서는 AWS Lambda에 대한 기본 설명을 제공합니다. 이벤트 중심 환경에서 람다 함수를 만드는 데 필요한 단계를 보여줍니다.

AWS Lambda 는 이벤트에 응답하여 코드를 실행하고 자동으로 컴퓨팅 리소스를 관리하는 컴퓨팅 서비스로 새로운 정보에 빠르게 응답하는 응용 프로그램을 쉽게 만들 수 있습니다. AWS Lambda는 이미지 업로드, 인앱 활동, 웹 사이트 클릭 또는 연결된 기기의 출력과 같은 이벤트가 밀리 초 이내에 실행되도록 합니다. AWS Lambda를 사용하여 사용자 정의 요청을 기반으로 컴퓨팅 리소스가 자동으로 트리거 되는 새로운 백엔드 서비스를 생성 할 수도 있습니다.


## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* AWS 람다 함수 만들기
* Amazon S3 버킷을 람다 이벤트 소스로 구성
* Amazon S3에 객체를 업로드하여 람다 함수를 트리거 하기
* Amazon CloudWatch Log를 통해 AWS Lambda S3 기능 모니터링


## Start Lab:

https://www.qwiklabs.com/ 에서 ”Introduction to AWS Lambda“를 검색해서 실습을 진행합니다.

**1. 상단 화면에  을 클릭하여 실습을 시작합니다. 그러면 LAB 리소스 프로비저닝 프로세스가 시작됩니다. LAB 리소스를 구성하는데 소요되는 예상시간이 표시됩니다. 다음을 진행하기 전에 리소스 프로비저닝이 완료될 때까지 기다려야 합니다.**

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



<br>

## 시나리오 :
이 실습에서는 Serverless 이미지 축소 응용프로그램을 만들어 AWS Lambda를 보여줍니다 .

다음 다이어그램은 응용 프로그램 흐름을 보여줍니다.\
![image](https://user-images.githubusercontent.com/48195985/61930674-6d5af980-afb9-11e9-9a4f-f3dd41ab6a2a.png)

> 1. 사용자가 Amazon S3 (객체 생성 이벤트) 의 소스 버킷에 객체를 업로드합니다.
> 2. Amazon S3가 객체 생성 이벤트를 감지합니다.
> 3. Amazon S3는 Lambda 함수를 호출하고 이벤트 데이터를 함수 매개 변수로 전달하여 객체 생성 이벤트를 AWS Lambda에 게시합니다.
> 4. AWS 람다는 람다 함수를 실행합니다.
> 5. 수신한 이벤트 데이터에서 람다 함수는 소스 버킷 이름과 객체 키 이름을 알고 있습니다. Lambda 함수는 객체를 읽고 그래픽 라이브러리를 사용하여 축소판을 만든 다음 축소판을 대상 버킷에 저장합니다.

이 실습을 완료하려면 계정에 다음 리소스가 제공되어야 합니다.

![image](https://user-images.githubusercontent.com/48195985/61930760-c1fe7480-afb9-11e9-9274-4d059b1b5a00.png)

> 이 실습의 단계에서는 Amazon S3 버킷 및 람다 함수를 만드는 방법을 보여줍니다. 그런 다음 크기 조정을 위해 이미지를 업로드하여 서비스를 테스트합니다.


<br>

## 작업 1 : Amazon S3 버킷 생성

**1. AWS 관리 콘솔에서 서비스를 클릭하고 S3를 클릭합니다.**

**2. S3 버킷을 생성하기 위해 ![image](https://user-images.githubusercontent.com/48195985/61931806-af396f00-afbc-11e9-870e-076b94d3bd6d.png)
를 클릭합니다.**
> * 버킷 이름 : image-_**NUMBER**_
> * _**NUMBER**_ 는 임의의 번호로 변경합니다.
> * 노트패드에 버킷이름을 복사해 놓습니다.
> * ![image](https://user-images.githubusercontent.com/48195985/61931772-992bae80-afbc-11e9-8eb2-87d37fe1a55d.png)을 클릭합니다.

> Amazon S3 버킷의 이름은 images-190717와 같은 고유한 이름이여야 합니다.
생성시 요청된 버킷 이름을 사용할 수 없다는 오류가 표시되면 버킷이름이 고유할 수 있도록 수정합니다.

> 이제 이미지 출력용으로 다른 버킷을 생성합니다.

**3. S3 버킷을 생성하기 위해 ![image](https://user-images.githubusercontent.com/48195985/61931806-af396f00-afbc-11e9-870e-076b94d3bd6d.png)를 클릭합니다.**
* 버킷 이름 : 이전에 생성한 버킷이름을 붙여넣기 합니다.
* 버킷이름 끝에 “**–resized**”를 추가합니다. (**images-190717-resized** 와 유사합니다.)
* ![image](https://user-images.githubusercontent.com/48195985/61934193-bcf1f300-afc2-11e9-94ff-9a4e52ac5bf6.png)을 클릭합니다.

>  <span style="color:red">**Region을 변경하지 마십시오.**</span>

> 생성된 버킷의 이름은 다름과 유사합니다.\
![image](https://user-images.githubusercontent.com/48195985/61934238-d6933a80-afc2-11e9-8eaa-aea4a70f199b.png)

**4. 다음 주소를 클릭하고 http://bit.ly/2XQ6o3K 사이트에 접속합니다.**
* 이미지를 선택한 다음 마우스 우측클릭으로 “**이미지를 다른 이름으로 저장(V)...**”을 누릅니다.
* 파일이름은 **happyFace.jpg**로 저장합니다.

**5. S3 관리 콘솔에서 images-_NUMBER_ 이름으로 처음 생성한 버킷을 클릭합니다.**

**6. ![image](https://user-images.githubusercontent.com/48195985/61934439-4275a300-afc3-11e9-851a-955a7bbb0b8f.png)
를 클릭합니다.**

**7. ![image](https://user-images.githubusercontent.com/48195985/61934454-499cb100-afc3-11e9-86bd-e6ed06f78eca.png)를 클릭합니다.**

**8. 4번 항목에서 다운로드한 HappyFace.jpg 파일을 선택합니다.**

**9. ![image](https://user-images.githubusercontent.com/48195985/61934473-51f4ec00-afc3-11e9-8112-5af1361ffa1b.png)를 클릭합니다.**

> 이 실습의 뒷부분에서 샘플 이벤트 데이터를 함수에 전달하여 Lambda 함수를 수동으로 호출합니다. 샘플 데이터는 HappyFace.jpg 이미지를 참조합니다.

<br>

## 작업 2 : AWS 람다 함수 만들기

이 작업에서는 Amazon S3에서 이미지를 읽고, 이미지의 크기를 조정 한 다음 Amazon S3에 새 이미지를 저장하는 AWS Lambda 함수를 작성합니다.

**10.  ![image](https://user-images.githubusercontent.com/48195985/61934510-72bd4180-afc3-11e9-92ed-c110042d1cc0.png)메뉴에서 Lambda를 클릭합니다.**

**11. ![image](https://user-images.githubusercontent.com/48195985/61934519-794bb900-afc3-11e9-9d15-22c4ed914690.png)를 클릭합니다.**

> “블루프린트 사용“은 람다 함수를 작성하기 위한 코드 템플릿입니다. ”블루프린트 사용“은 Alexa 기술 제작 및 Amazon Kinesis Firehose 스트림 처리와 같은 표준 Lambda 트리거에 제공됩니다. 이 실습에서는 사전 작성된 람다 함수를 제공하므로 새로 작성합니다.

**12. 함수생성 창에서 다음을 구성합니다.**
> * 함수이름 : **Create-Thumbnail**
> * 런타임 : **Python 3.7**
> * 실행 역할 : **기존 역할 사용**
> * 기존 역할 : **lambda-execution-role**

> ![image](https://user-images.githubusercontent.com/48195985/61934567-98e2e180-afc3-11e9-8d95-21f92090e433.png)

**13.  ![image](https://user-images.githubusercontent.com/48195985/61934693-e8c1a880-afc3-11e9-82d2-c213c0ec66f4.png)
을 클릭합니다.**

> 기능 구성과 함께 페이지가 표시됩니다.\
![image](https://user-images.githubusercontent.com/48195985/61934711-f1b27a00-afc3-11e9-94f3-90892eb8501a.png)

> AWS Lambda 기능은 Amazon Kinesis가 수신하는 데이터 또는 Amazon DynamoDB 데이터베이스에서 업데이트 되는 데이터와 같은 활동에 의해 자동으로 트리거 될 수 있습니다. 이 실습에서는 Amazon S3 버킷에 새 객체가 만들어질 때마다 람다 함수를 트리거합니다.
**14.  ![image](https://user-images.githubusercontent.com/48195985/61934769-0db61b80-afc4-11e9-9253-4b23089aacba.png)
를 누르고 S3를 추가합니다.**

**15. 트리거 구성에서 다음 내용을 구성합니다.**
> * 버킷 : **images-NUMBER** (_버킷이름의 NUMBER는 위의 실습에서 생성한 번호임_.)
> * 이벤트 유형 : **모든 객체 생성 이벤트**
> 
> ![image](https://user-images.githubusercontent.com/48195985/61934796-1a3a7400-afc4-11e9-9bd1-d0492bbce86f.png)

**16.  ![image](https://user-images.githubusercontent.com/48195985/61934919-5b328880-afc4-11e9-87dd-4b08e869f38f.png)를 클릭합니다.**


**17. 디자이너 섹션 상단의 ![image](https://user-images.githubusercontent.com/48195985/61934948-684f7780-afc4-11e9-9fd6-3e5698559921.png)을 클릭합니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61934983-78675700-afc4-11e9-9f46-22a462b5cee5.png)

> 이제 람다 함수를 구성합니다.

**18. 스크롤을 내려 화면 아래의  ”함수 코드“ 섹션에 다음 설정을 구성합니다**.

> * 코드입력유형 : Amazon S3에서 파일 업로드
> * 런타임 : Python 3.7
> * 핸들러 : CreateThumbnail.handler
> * Amazon S3 링크 URL : https://s3-us-west-2.amazonaws.com/us-west-2-aws-training/awsu-spl/spl-88/scripts/CreateThumbnail.zip
> 
> **※ 핸들러 필드를 위의 값으로 설정 하십시오. 그렇지 않으면 람다 함수를 찾을 수 없습니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61935160-deec7500-afc4-11e9-8784-16a60a9f86d2.png)

> ※ Amazon S3 링크 URL을 통해 다운로드 받은 zip 파일의 내부 코드는 아래와 같습니다. (참고)

```python
import boto3
import os
import sys
import urllib
from PIL import Image
import PIL.Image

s3_client = boto3.client('s3')

def resize_image(image_path, resized_path):
    with Image.open(image_path) as image:
        image.thumbnail((128,128))
        image.save(resized_path)

def handler(event, context):
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        raw_key = urllib.parse.unquote_plus(key)
        download_path = '/tmp/{}'.format(key)
        upload_path = '/tmp/resized-{}'.format(key)

        s3_client.download_file(bucket, raw_key, download_path)
        resize_image(download_path, upload_path)
        s3_client.upload_file(upload_path,
             '{}-resized'.format(bucket),
             'thumbnail-{}'.format(raw_key),
             ExtraArgs={'ContentType': 'image/jpeg'})
```

**19.  위의 코드를 확인한 후 다음 단계를 수행합니다.**
> * 들어오는 객체 (Bucket, Key)의 이름을 포함하는 이벤트를 받습니다.
> * 이미지를 로컬 저장소로 다운로드 합니다.
> * Pillow 라이브러리를 사용하여 이미지의 크기를 조정합니다.
> * 크기가 조정된 이미지는 –resized 버킷에 업로드 됩니다.


**20. ”기본 설정“ 섹션에서 설명을 다음으로 입력합니다.**
> 설명 : Create a thumbnail-sized image

> **다른 설정은 기본값으로 두지 만 다음은 이러한 설정에 대한 간단한 설명입니다.**
> * 메모리는 함수에 할당할 리소스를 정의합니다. 메모리를 늘리면 함수에 할당된 CPU도 증가합니다.
> * 제한 시간은 함수 실행의 최대 지속 기간을 설정합니다.
> * VPC(Virtual Private Cloud) 설정은 지정한 VPC내의 리소스에 대한 람다 기능 액세스를 제공합니다.
> * DLQ(Dead Letter Queue) 리소스는 실패한 함수 실행을 처리하는 방법을 정의합니다.
> * 액티브 추적 활성화를 사용하면 AWS X-Ray를 통해 분산 코드를 추적하고 모니터링 할 수 있습니다.

**21. 화면 우측 상단의 ![image](https://user-images.githubusercontent.com/48195985/61935326-3985d100-afc5-11e9-9c95-a07e4283ca50.png)
을 클릭합니다.**

> 이제 Lambda 함수가 정의되었습니다.

<br>


## 작업 3 : 함수 테스트 하기

이 작업에서는 람다 함수를 테스트합니다. 이는 새로운 객체가 업로드 될 때 Amazon S3에서 일반적으로 전송되는 것과 동일한 정보로 이벤트를 시뮬레이트 함으로써 수행됩니다.

**22. 화면 상단에서 ![image](https://user-images.githubusercontent.com/48195985/61935361-50c4be80-afc5-11e9-910b-55dbaed47c05.png)를 클릭한 후 “테스트 이벤트 구성”창이 뜨면 다음을 구성합니다.**
* 이벤트 템플릿 : Amazon S3 Put
* 이벤트 이름 : Upload

> 아래에 샘플 템플릿이 표시되어 Amazon S3으로 업로드하여 람다 함수가 이벤트 데이터를 트리거 할 때 이벤트 데이터를 표시합니다. 이전에 생성 한 버킷을 사용하도록 버킷 이름을 편집해야 합니다.

> ![image](https://user-images.githubusercontent.com/48195985/61935443-8073c680-afc5-11e9-911f-c1e4d7e6205b.png)

> 위 그림에서 표시된 부분의 코드를 다음 내용으로 변경합니다.

* “example-bucket” 은 images-NUMBER (S3에 생성한 버킷이름)로 변경합니다.
* “test/key” 는 HappyFace.jpg (다운로드 받은 이미지 파일이름)으로 변경합니다.

**23.  ![image](https://user-images.githubusercontent.com/48195985/61935496-9aada480-afc5-11e9-8933-73eaa87bf1fd.png)화면 하단의 을 클릭합니다.**

**24.  화면 우측 상단의  ![image](https://user-images.githubusercontent.com/48195985/61935508-a13c1c00-afc5-11e9-82ca-b207e3e0d81d.png)를 클릭합니다.**

> 화면 맨 위로 스크롤합니다. 화면 상단에 다음 메시지가 표시되어야 합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/61935559-b5801900-afc5-11e9-8690-6879dd038e47.png)

> 테스트가 성공하지 못하면 오류 메시지가 실패 원인을 설명합니다. 예를 들어 “**Access Deny**” 메시지는 잘못된 버킷 이름으로 인해 이미지를 찾지 못했음을 의미합니다. 이전 단계를 검토하여 버킷이름 과 파일이름을 올바르게 구성했는지 확인하십시오.
 
**25. 상단 메시지 부분의 ![image](https://user-images.githubusercontent.com/48195985/61935598-cd579d00-afc5-11e9-8fb5-949c704733aa.png)
를 클릭해서 상세 내용을 표시합니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61935708-098afd80-afc6-11e9-8f7e-afa9fe9f90d1.png)

> 다음과 같은 정보가 표시됩니다.
> 
> * 실행 기간
> * 구성된 리소스
> * 사용된 최대 메모리
> * 로그 출력
> 
> 이제 Amazon S3에 저장된 크기 조정된 이미지를 볼 수 있습니다.

**26.  ![image](https://user-images.githubusercontent.com/48195985/61935735-19a2dd00-afc6-11e9-8270-b156f2cab3f5.png)메뉴에서 S3를 클릭합니다.**

**27. 두 번째로 생성한 버킷이름(images-190718-resized 와 유사함)을 클릭하고 다음을 수행합니다.**
* thumbnail-HappyFace.jpg를 클릭합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61935796-34755180-afc6-11e9-9f35-1af2326dec3a.png)를 클릭합니다. (이미지가 열리지 않으면 팝업 차단을 해제 하십시오.)
* 만약 웹페이지에서 열리지 않고 다운로드 되면 다운로드된 파일을 클릭해서 엽니다.
* 열린 이미지는 작게 축소된 이미지이여야 합니다.

<span style="color:red">**※ 다른 이미지를 images-NUMBER 버킷에 업로드 하고 축소판이 images-NUMBER-resized 버킷에 저장되는지 확인해 봅니다.**

<br>

## 작업 4 : 모니터링 및 로깅

AWS Lambda 함수를 모니터링하여 문제를 식별하고 디버깅을 지원하는 로그 파일을 볼 수 있습니다.

**28.  ![image](https://user-images.githubusercontent.com/48195985/61935735-19a2dd00-afc6-11e9-8270-b156f2cab3f5.png)메뉴에서 Lambda를 클릭합니다.**

**29. 함수이름 아래  ![image](https://user-images.githubusercontent.com/48195985/61935912-74d4cf80-afc6-11e9-84d6-8b29e8e798bb.png)
을 클릭합니다.**

**30. 모니터링 탭을 클릭합니다.**
> 콘솔에 다음과 같은 그래프가 표시됩니다.\
> ![image](https://user-images.githubusercontent.com/48195985/61935934-8918cc80-afc6-11e9-8ea2-772d09ec5046.png)

* Invocations : 함수가 호출 된 횟수입니다.
* Duration : 함수 실행에 걸린 시간 (밀리 초).
* Error : 함수가 실패한 횟수입니다.
* Throttles : 너무 많은 기능을 동시에 호출하면 스로틀 됩니다. 기본값은 1000개의 동시 실행입니다.
* Iterator Age : 스트리밍 트리거 (Amazon Kinesis 및 Amazon DynamoDB 스트림)에서 처리된 마지막 레코드의 나이를 측정합니다.
* Dead Letter Errors : 오류 발생 시 DLQ 대기열에 메시지를 보낸 정보

**람다 함수의 로그 메시지는 Amazon CloudWatch 로그에 유지됩니다 .**

**31. ![image](https://user-images.githubusercontent.com/48195985/61935993-b2395d00-afc6-11e9-84c6-a785424b951b.png)를 클릭합니다.**

**32. 출력된 로그 스트림을 클릭합니다. (아래 이미지와 유사합니다.)**

> ![image](https://user-images.githubusercontent.com/48195985/61936010-bfeee280-afc6-11e9-965a-a60dea1ab0b3.png)

**33. 로그 메시지 왼쪽에 ![image](https://user-images.githubusercontent.com/48195985/61936092-f6c4f880-afc6-11e9-96d2-7a7394748a63.png)
를 클릭하면 메시지가 확장되어 세부내용을 확인할 수 있습니다.**

> 이벤트 데이터에는 Requestid, Duration, Billed Duration, Memory Size 필드가 표시됩니다. 


<br>

## 실습을 완료합니다.
