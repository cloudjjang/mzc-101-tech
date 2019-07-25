# 실습 : Introduction to Amazon Relational Database Service (RDS) (Linux)

## LAB Overview:

이 실습에서는 AWS Management Console을 사용하여 Amazon RDS를 소개합니다.

Amazon RDS(Relational Database Service)는 클라우드에서 관계형 데이터베이스를 쉽게 설정, 작동 및 확장 할 수 있게 해주는 웹 서비스입니다. MySQL, PostgreSQL, Oracle 또는 Microsoft SQL Server 데이터베이스를 작성하고 사용할 수 있습니다. 즉, 기존 데이터베이스에서 이미 사용하고 있는 코드, 응용 프로그램 및 도구를 Amazon RDS와 함께 사용할 수 있습니다.

## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* CloudFormation을 이용한 실습환경 구성하기
* Amazon RDS 인스턴스 만들기
* 클라이언트 소프트웨어를 사용하여 RDS 인스턴스에 연결

## Start Lab:

* 크롬 웹 브라우저를 실행합니다.
* 주소창에 다음 주소를 입력합니다. https://aws.amazon.com 
* 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61847698-9a8aa780-aee6-11e9-9751-16c61c4da1fd.png)을 누르고 자신의 계정으로 로그인 합니다.
* Region은 오레콘(Oregon)을 선택합니다.


## 작업 1 : 실습 환경 구성하기

**1. AWS 관리 콘솔에서 서비스를 클릭하고 EC2를 클릭합니다.**

**2. 왼쪽 탐색창에서 “키 페어”를 클릭합니다.**
* 화면 상단의 ![image](https://user-images.githubusercontent.com/48195985/61847714-aecea480-aee6-11e9-87e7-9504e8004b36.png)
을 클릭합니다.
* 키 페어 이름 : **mzc-lab-key**을 입력하고 ![image](https://user-images.githubusercontent.com/48195985/61847742-c148de00-aee6-11e9-9f6c-95fb0a6ce995.png)을 누릅니다.
* 생성된 키페어의 pem 파일 다운로드 창이 뜨면 “저장”을 누르고 다운로드 된 mzc-lab-key.pem 파일을 바탕화면에 보관합니다.


**3. 새 웹 웹 브라우저를 열고 다음 주소를 입력한다.**
* 주소 : http://bit.ly/2YawLAz 를 클릭합니다.
* 주소를 클릭하면 lab04.cf-rds.yml 파일이 다운로드 됩니다.
* 바탕화면에 이 파일을 저장합니다.


**4. AWS 관리콘솔에서 ![image](https://user-images.githubusercontent.com/48195985/61847853-21d81b00-aee7-11e9-88fd-2054d6e14527.png)를 누르고 “CloudFormation”을 클릭합니다.**


**5. ![image](https://user-images.githubusercontent.com/48195985/61847879-33212780-aee7-11e9-93c3-86cf4f3688e5.png)
을 클릭하고 다음으로 구성합니다.**
* 템플릿 준비에서 ![image](https://user-images.githubusercontent.com/48195985/61847890-446a3400-aee7-11e9-9eea-96ccedbf81df.png)을 선택합니다.
* 템플릿 지정에서 ![image](https://user-images.githubusercontent.com/48195985/61847915-4d5b0580-aee7-11e9-8a0d-2ecad8becc83.png)를 선택합니다.
* 템플릿 파일 업로드에서 ![image](https://user-images.githubusercontent.com/48195985/61847925-55b34080-aee7-11e9-9ec5-c25204b6cf69.png)을 클릭하고 이전에 다운로드 받은 lab04.cf-rds.yml 파일을 엽니다.
* 화면 하단의 ![image](https://user-images.githubusercontent.com/48195985/61847936-62379900-aee7-11e9-9de0-2e02097f31d8.png)
을 클릭합니다.


**6. 스택 세부 정보 지정 창에서 다음을 입력합니다.**
* 스택이름 : mzc-lab
* ![image](https://user-images.githubusercontent.com/48195985/61847936-62379900-aee7-11e9-9de0-2e02097f31d8.png)
을 클릭합니다.


**7. 스택 옵션 구성 창에서 스크롤을 아래로 내려서 ![image](https://user-images.githubusercontent.com/48195985/61847936-62379900-aee7-11e9-9de0-2e02097f31d8.png)을 클릭합니다.**

**8. mzc-lab 검토 창에서 스크롤을 아래로 내려서 ![image](https://user-images.githubusercontent.com/48195985/61847879-33212780-aee7-11e9-93c3-86cf4f3688e5.png)을 클릭합니다.**

**9. mzc-lab 스택생성 창에서 “이벤트” 탭을 선택하고  ![image](https://user-images.githubusercontent.com/48195985/61848328-b55e1b80-aee8-11e9-9dca-965552e3184a.png)눌러 이벤트를 갱신합니다. (30초마다 누름)**

**10. 죄측 창에 스택 상태가 ![image](https://user-images.githubusercontent.com/48195985/61848337-bee78380-aee8-11e9-8a25-794ddcc59a79.png)이 될 때까지 기다립니다.**
> 최종 완료된 화면은 아래와 같습니다.\
![image](https://user-images.githubusercontent.com/48195985/61848356-cc047280-aee8-11e9-8073-3cad21bbf46d.png)

<span style="color:red">**실습환경 구성이 완료되었습니다.**</span>


## **실습 토폴로지 :**

![image](https://user-images.githubusercontent.com/48195985/61848464-200f5700-aee9-11e9-8ac2-3282ca8a14bb.png)


## 작업 2 : Amazon RDS 인스턴스 생성하기

**11. AWS관리 콘솔 상단의   ![image](https://user-images.githubusercontent.com/48195985/61848497-41704300-aee9-11e9-9988-44b8e857e6fa.png)메뉴에서 “RDS”를 클릭합니다.**

**12. 왼쪽 탐색창에서 “데이터베이스”를 클릭합니다.**

**13. 화면 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61848523-577e0380-aee9-11e9-929a-466ce5c7c70b.png)을 클릭합니다.**
> 데이터베이스 엔진을 선택합니다. Amazon RDS는 다양한 데이터베이스와 각 데이터베이스의 여러 버전을 지원합니다. 이 실습에서는 MySQL을 사용합니다.


**14. 1단계 : 엔진 선택**
* 엔진옵션에서 “**MySQL**”을 선택합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61848569-809e9400-aee9-11e9-985b-4260d893da7f.png)를 클릭합니다.


**15. 2단계 : 사용사례 선택**
* **개발/테스트 – MySQL** 선택
* ![image](https://user-images.githubusercontent.com/48195985/61848569-809e9400-aee9-11e9-985b-4260d893da7f.png)를 클릭합니다.


**16. 3단계 : DB세부 정보 지정**
* DB 인스턴스 클래스 항목을 “**db.t2.micro – 1vCPU, 1GiB RAM**”로 선택합니다.
* 아래로 스크롤을 내려 설정에 다음을 구성합니다.
* DB인스턴스 식별자 : **my-rds**
* 마스터 사용자 이름 : **student**
* 마스터 엄호 : **Pass.123**
* 암호 확인 : **Pass.123**
* ![image](https://user-images.githubusercontent.com/48195985/61848569-809e9400-aee9-11e9-985b-4260d893da7f.png)를 클릭합니다.


**17. 4단계 : 고급설정 구성**
* Virtual Private Cloud(VPC) : **Lab VPC**를 선택합니다.
* 서브넷 그룹 : **db-subnet-group**을 선택합니다.
* VPC 보안 그룹 : **기존 VPC 보안 그룹 사용**을 선택하고 다음을 구성합니다.
* **mzc-lab-DbSecurityGroup**을 선택합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61848657-d07d5b00-aee9-11e9-85b2-2627d2ae5582.png)
를 삭제합니다. (default 옆 X를 클릭하면 삭제됨)

**18. 데이터베이스 옵션 섹션에서 “데이터베이스 이름”항목에 lab을 입력합니다.**

**19. 백업 섹션에서 백업 보존 기간을 “0일”로 선택합니다.** 
> ![image](https://user-images.githubusercontent.com/48195985/61848705-fdca0900-aee9-11e9-9892-ae7eadd55ca7.png)

**20. 스크롤을 마지막까지 내린다음 ![image](https://user-images.githubusercontent.com/48195985/61848755-22be7c00-aeea-11e9-9e96-3bfe3776c3c3.png)
을 클릭합니다.**

**21. ![image](https://user-images.githubusercontent.com/48195985/61848771-2e11a780-aeea-11e9-801c-3304081710d5.png)를 클릭합니다.**

> 이 페이지에는 새로 생성된 RDS 인스턴스의 세부 정보가 표시됩니다. RDS 인스턴스를 만드는 데 약 10 분이 소요됩니다.

> <span style="color:red">**다음 작업을 계속하십시오. 데이터베이스가 시작될 때까지 기다릴 필요가 없습니다.**</span>

<br>

## 작업 3 : EC2 인스턴스에 로그인하기

Lab 구성하는 CloudFormation 스택생성시에 Amazon EC2 Linux 인스턴스가 만들어졌습니다. 이 인스턴스에 로그인 합니다.

**22. ![image](https://user-images.githubusercontent.com/48195985/61848497-41704300-aee9-11e9-9988-44b8e857e6fa.png)를 클릭하고 EC2를 선택합니다.**

**23. 왼쪽 탐색창에서 “인스턴스”를 클릭합니다.**

**24. ‘bastion“ 인스턴스를 선택하고 상위에 ![image](https://user-images.githubusercontent.com/48195985/61848919-919bd500-aeea-11e9-8077-009fa852613d.png)을 누릅니다.**

**25. 인스턴스 연결 창에서 다음을 구성합니다.**
* ![image](https://user-images.githubusercontent.com/48195985/61848934-9d879700-aeea-11e9-879d-d09f3c54ddeb.png)
를 선택합니다.
* ![image](https://user-images.githubusercontent.com/48195985/61848944-a6786880-aeea-11e9-9ce4-5287b6c7a63b.png)를 클릭합니다.
* 팝업창이 뜨면서 인스턴스 콘솔창에 접속한 화면이 나타납니다.\
![image](https://user-images.githubusercontent.com/48195985/61848953-aed0a380-aeea-11e9-986b-db641691efe6.png)


<br>


## 작업 4 : 데이터베이스에 액세스 하기

이제 EC2 인스턴스에 설치된 mysql 클라이언트 를 사용하여 RDS 데이터베이스에 연결 합니다.
먼저 연결 세부 정보를 수집하여 새 연결을 만듭니다.

**26. AWS 관리 콘솔로 돌아갑니다.**

**27. ![image](https://user-images.githubusercontent.com/48195985/61848497-41704300-aee9-11e9-9988-44b8e857e6fa.png)를 클릭하고 RDS를 선택합니다.**

**28. 왼쪽 탐색창에서 “데이터베이스”를 클릭합니다.**

**30. my-rds 데이터베이스 항목에 상태가 ![image](https://user-images.githubusercontent.com/48195985/61849000-db84bb00-aeea-11e9-9b54-d18184e391ba.png)이 될 때 까지 기다립니다. ![image](https://user-images.githubusercontent.com/48195985/61849014-eb040400-aeea-11e9-9c67-9a46ed07c8b0.png)(새로고침)를 눌러 정기적으로 확인합니다.**

**31. “my-rds” 데이터베이스를 클릭합니다.**

**32. ![image](https://user-images.githubusercontent.com/48195985/61849042-00792e00-aeeb-11e9-90fe-530484f1d8ea.png)탭에서 엔드포인트 주소를 복사해서 노트패드에 복사해 놓습니다.**\
> 다음 주소와 유사합니다 :\
> my-rds.cfiqursnpryu.us-west-2.rds.amazonaws.com

**33. EC2 인스턴스에 접속한 SSH 터미널창으로 이동하여 다음을 입력합니다.**

``` bash
 $ mysql --user student --password --host ENDPOINT
```

* ENDPOINT는 노트패드에 복사해 놓은 주소로 변경합니다.
* Enter를 입력합니다.
* Enter password : **Pass.123** 암호를 입력합니다. <span style="color:red">아래와 같이 나타납니다.</span>
![image](https://user-images.githubusercontent.com/48195985/61849077-15ee5800-aeeb-11e9-9d92-0277befad6b9.png)



**34. 다음 명령을 입력해서 새 테이블을 만들고 데이터를 입력합니다.**

``` sql
CREATE TABLE lab.staff (firstname text, lastname text, phone text);
INSERT INTO lab.staff VALUES (“john”, “Smith”, “555-1234”);
INSERT INTO lab.staff VALUES (“Sarah”, “Jones”, “5555-8866”);
```


**35. 다음 명령을 입력하면 “Sarah”의 세부정보를 출력합니다.**
``` sql
select * from lab.staff where firstname = “Sarah”;
```

<span style="color:red">**실습을 종료합니다.** </span>

<span style="color:red">**※ 생성된 RDS를 삭제하고 CloudFormation 스택도 삭제합니다.**</span>
