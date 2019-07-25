# 실습 : Introduction to Amazon DynamoDB

## LAB Overview:

Amazon DynamoDB 는 모든 규모에서 일관된 한 자릿수의 밀리 초 대기 시간을 필요로 하는 모든 애플리케이션에 대해 빠르고 유연한 NoSQL 데이터베이스 서비스입니다. 이 데이터베이스는 완벽하게 관리 데이터베이스 및 문서 및 키 - 값 데이터 모델을 모두 지원합니다. 유연한 데이터 모델과 안정적인 성능으로 모바일, 웹, 게임, 광고 기술, IoT 및 기타 여러 애플리케이션에 이상적입니다.

이 실습에서는 Amazon DynamoDB에 테이블을 만들어 음악 라이브러리에 대한 정보를 저장합니다. 그런 다음 음악 라이브러리를 쿼리하고 마지막으로 DynamoDB 테이블을 삭제합니다.


## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* Amazon DynamoDB 테이블 만들기
* Amazon DynamoDB 테이블에 데이터 입력
* Amazon DynamoDB 테이블 쿼리
* Amazon DynamoDB 테이블 삭제


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

<br>

## 작업 1 : 새 테이블 만들기

이 작업에서는 DynamoDB에 Music이라는 새 테이블을 만듭니다. 각 테이블에는 DynamoDB 서버에서 데이터를 분할하는 데 사용되는 기본 키가 필요합니다. 테이블에는 정렬 키가 있을 수도 있습니다. 기본 키와 정렬 키의 조합은 DynamoDB 테이블의 각 항목을 고유하게 식별합니다.

**1. AWS 관리 콘솔에서 ![image](https://user-images.githubusercontent.com/48195985/61849734-30c1cc00-aeed-11e9-837f-87223c836676.png)를 클릭하고 DynamoDB를 클릭합니다.**

**2. ![image](https://user-images.githubusercontent.com/48195985/61849724-26073700-aeed-11e9-8cba-a846ecb53961.png)를 클릭합니다.**

**3. DynamoDB 테이블 만들기 단계에서 다음을 입력합니다.**
* 테이블 이름 : **Music**
* 기본 키 : 파티션키에 **Artist**를 입력하고 “**문자열**”을 선택합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61849786-5fd83d80-aeed-11e9-9cef-f18ef28fbce1.png)를 선택하고 **Song**을 입력하고 “**문자열**”을 선택합니다.

> 테이블은 인덱스 및 프로비저닝 된 용량에 대한 기본 설정을 사용합니다.

**4. ![image](https://user-images.githubusercontent.com/48195985/61849822-7d0d0c00-aeed-11e9-99cd-c8960109d857.png)을 클릭합니다. (테이블은 1분 안에 생성됩니다.)**

<br>

## 작업 2 : 데이터 추가하기

이 작업에서는 Music 테이블에 데이터를 추가합니다. 테이블은 특정 주제에 대한 데이터의 모음입니다.

각 테이블에는 여러 항목이 있습니다. 항목은 다른 모든 항목에서 고유하게 식별 할 수 있는 속성 그룹입니다. DynamoDB의 항목은 여러 가지면에서 다른 데이터베이스 시스템의 행과 비슷합니다. DynamoDB에서 테이블에 저장할 수 있는 항목 수에는 제한이 없습니다.

각 항목은 하나 이상의 속성으로 구성 됩니다. 속성은 기본 데이터 요소로, 더 이상 분해 할 필요가 없습니다. 예를 들어, 음악 테이블의 항목에는 노래 및 아티스트와 같은 속성이 포함됩니다. DynamoDB의 속성은 다른 데이터베이스 시스템의 비슷한 열이지만 각 항목 (행)은 다른 속성 (열)을 가질 수 있습니다.

DynamoDB 테이블에 항목을 쓸 때 기본 키와 정렬 키 (사용된 경우)만 필요합니다. 이 필드 이외의 테이블에는 스키마가 필요하지 않습니다. 즉, 다른 항목의 속성과 다른 속성을 하나의 항목에 추가 할 수 있습니다.

**5. ![image](https://user-images.githubusercontent.com/48195985/61849869-a037bb80-aeed-11e9-85ee-be639822467e.png)탭을 클릭하고 ![image](https://user-images.githubusercontent.com/48195985/61849882-a62d9c80-aeed-11e9-955f-6afe869970ef.png)를 클릭합니다.**

**6. 항목 만들기 창에 다음을 입력합니다.**
* Artist String : **Pink Ployd**
* Song String : **Money**\
![image](https://user-images.githubusercontent.com/48195985/61849911-c52c2e80-aeed-11e9-940a-223bff434ff2.png)


**7. 속성을 추가하기 위해 Song 왼쪽에 ![image](https://user-images.githubusercontent.com/48195985/61849970-f1e04600-aeed-11e9-8fcb-9baac6faddf4.png)키를 클릭합니다.**
> ![image](https://user-images.githubusercontent.com/48195985/61849985-045a7f80-aeee-11e9-9901-46395bf59815.png)


**8. 드롭 다운 목록에서 “String”을 선택합니다.**
> 새로운 속성 행이 추가됩니다.

**9. 새 속성에 다음을 입력합니다.**
* FIELD : **Album**
* VALUE : **The Dark Side of the Moon**

**10. Album 옆의 ![image](https://user-images.githubusercontent.com/48195985/61849970-f1e04600-aeed-11e9-8fcb-9baac6faddf4.png)키를 눌러 새 속성을 추가합니다.** 

**11. 드롭 다운 목록에서 Number를 클릭하고 속성행이 추가되면 다음을 입력합니다.**
* FIELD : **Year**
* VALUE : **1973**

**입력이 완료된 화면은 아래와 같습니다.**\
![image](https://user-images.githubusercontent.com/48195985/61850132-70d57e80-aeee-11e9-889a-612b02d0ad72.png)

**12. ![image](https://user-images.githubusercontent.com/48195985/61850156-8054c780-aeee-11e9-81f1-477c8ed319ce.png)
을 클릭합니다.**

**13. 다음과 같이 항목이 콘솔에 표시됩니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61850168-8cd92000-aeee-11e9-8681-e20f511bfd55.png)

**14. 다음 속성을 사용하여 두 번째 항목(Item)을 생성합니다.**

|Attribute Name|Attribute Type|Attribute Valu|
|---|---|---|
|Artist|String|John Lennon|
|Song|String|Imagine|
|Album|String|Imagine|
|Year|Number|1971|
|Genre|String|Soft rock|


> 이 항목(Item)은 “Genre”라는 추가 속성이 있습니다. 이것은 테이블 스키마를 미리정의 할 필요 없이 다른 속성을 가질 수 있는 각 항목의 예입니다.

**15. 세 번째 항목(Item)을 생성합니다.**

Attribute Name|Attribute Type|Attribute Value
---|---|---
Artist|String|Psy
Song|String|Gangnam Style
Album|String|Psy 6 (Sx Rules), Part 1
Year|Number|2011
LengthSeconds|Number|219

> 다시 한번이 항목에는 노래의 길이를 나타내는 새로운 LengthSeconds 속성이 있습니다. 이는 NoSQL 데이터베이스의 유연성을 보여줍니다.

> 또한 AWS 데이터 파이프 라인 사용, 프로그래밍 방식으로 데이터로드 또는 인터넷에서 사용 가능한 무료 도구 중 하나를 사용하여 DynamoDB에 데이터를 로드하는 더 빠른 방법이 있습니다.

<br>

## 작업 3 : 기존 항목(Item) 변경하기
입력한 데이터에 오류가 있음을 알게되었습니다. 기존 항목을 수정합니다.

**16. 항목이름인 “Psy”를 클릭합니다.**

**17. Year 속성의 값을 2011에서 2012로 변경합니다.**

**18. ![image](https://user-images.githubusercontent.com/48195985/61852404-5900f900-aef4-11e9-9aab-5c8dac31ebf7.png)을 클릭합니다.**


<br>

## 작업 4 : 테이블 쿼리

DynamoDB 테이블을 쿼리하는 두 가지 방법이 있습니다 : 쿼리 및 검색

쿼리 작업은 기본 키와 선택적으로 정렬 키에 따라 항목을 찾습니다. 완전히 인덱싱 되므로 매우 빠르게 실행됩니다.

**19. “스캔”으로 표시된 드롭 다운 목록을 클릭하고 “쿼리”로 변경합니다.**
> 파티션 키(기본 키와 동일 함) 및 정렬 키 필드가 표시됩니다.\
> ![image](https://user-images.githubusercontent.com/48195985/61852440-7766f480-aef4-11e9-8712-c5d0d90f6188.png)

**20. 세부 정보를 다음값으로 입력합니다.**
* 파티션 키 : **Psy**
* 정렬 키 : **Gangnam Style**
* ![image](https://user-images.githubusercontent.com/48195985/61852484-8f3e7880-aef4-11e9-9a22-ecc3c99e1464.png)을 클릭합니다.

> ![image](https://user-images.githubusercontent.com/48195985/61852534-ab421a00-aef4-11e9-9083-0a264f86b718.png)

> 노래가 목록에 빠르게 나타납니다. 쿼리는 DynamoDB의 테이블에서 데이터를 검색 할 수 있는 가장 효율적인 방법입니다.

> 또는 항목을 검색 할 수 있습니다. 이것은 테이블의 모든 항목을 조사하므로 덜 효율적이며 큰 테이블에는 상당한 시간이 걸릴 수 있습니다.

**21. “쿼리”가 표시된 드롭 다운 목록을 “스캔”으로 변경합니다.**

22. ![image](https://user-images.githubusercontent.com/48195985/61852628-de84a900-aef4-11e9-9b97-52324ba0efe5.png)
를 클릭하고 다음을 구성합니다.

* 속성 : **Year**
* 문자열을 **번호**로 변경
* Value : **1971**
* ![image](https://user-images.githubusercontent.com/48195985/61852484-8f3e7880-aef4-11e9-9a22-ecc3c99e1464.png)을 클릭합니다.

> 1971년에 발표된 노래만 표시됩니다.

<br>

## 작업 5 : 테이블 삭제

이 작업에서는 Music 테이블을 삭제하고 테이블의 모든 데이터도 삭제합니다.

**23. “테이블 삭제”를 클릭합니다. 테이블 삭제 확인창이 나오면 “삭제”를 클릭합니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61852704-12f86500-aef5-11e9-8216-14c378b6c4f0.png)\
> 테이블이 삭제됩니다.

<br>

## <span style="color:red">실습을 종료합니다.</span>
