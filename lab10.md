# 실습 : Building and Deploying Containers Using Amazon Elastic Container Service

## LAB Overview:

이 실습은 https://ecsworkshop.com 사이트의 워크샵을 기반으로 생성된 실습입니다.
Fargate, ECS 및 Docker 컨테이너 작업 과정을 실습을 통해 학습하게 됩니다.

**Three Tier 아키텍처** :
이 실습에서는 조직에서 업무를 위해 배치하는 전형적인 Three Tier 아키텍처 구조의 Microservice 스택을 구축해 봅니다.

![image](https://user-images.githubusercontent.com/48195985/62110806-2edf7a80-b2ea-11e9-86ec-0040816e0649.png)

 **CI / CD 파이프라인** :
모범 사례를 사용하여 일반적인 DevOps CI/CD 파이프 라인을 구축하고 Production 환경에 대한 수동 승격과 수용 환경에 대한 코드 업데이트를 지속적으로 제공합니다.

![image](https://user-images.githubusercontent.com/48195985/62110849-4880c200-b2ea-11e9-8b7b-358a656fd513.png)

## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* ECR을 사용하여 각 마이크로 서비스에 대한 컨테이너 레지스트리 구축
* CodePipeline 및 CodeBuild를 사용하여 각 마이크로 서비스에 대한 CI / CD 작성
* ECS를 사용하여 애플리케이션을 위한 수용 및 생산 환경 구축
* 업계 모범 사례를 사용하여 3 단계 다국어 응용 프로그램 배포
* 각 마이크로 서비스에 대한 자동 확장 정책 설정
* 각 서비스에 대해 구성된 블루 / 그린 배포
* CloudWatchLogs에 대한 각 마이크로 서비스의 중앙 집중식 로그
* CloudFormation을 사용하여 코드로 인프라를 구성

## Start Lab:

* 크롬 웹 브라우저를 실행합니다.
* 주소창에 다음 주소를 입력합니다. https://aws.amazon.com 
* 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/62111004-98f81f80-b2ea-11e9-885e-a077030eca81.png)을 누르고 자신의 계정으로 로그인 합니다.
* Region은 오레콘(Oregon)을 선택합니다.

<br>

## 작업 1 : 워크샵을 위한 사전 준비 단계

**1. Workship을 위한 IAM 사용자 계정 생성하기**

> 가)  ![image](https://user-images.githubusercontent.com/48195985/62111190-06a44b80-b2eb-11e9-8c17-7b41830efd27.png)메뉴에서 **IAM**을 클릭합니다.

> 나) 좌측 탐색창에서 “**사용자**”를 클릭합니다.

> 다) 상단의 “**사용자 추가**” 버튼을 클릭합니다.

> 라)  ![image](https://user-images.githubusercontent.com/48195985/62111068-bcbb6580-b2ea-11e9-9482-ebac0b849596.png)버튼을 클릭합니다.

> 마) 다음 내용으로 설정합니다.\
![image](https://user-images.githubusercontent.com/48195985/62111083-c5ac3700-b2ea-11e9-951a-0b6b869d6ac7.png)\

> 바) 화면 아래  ![image](https://user-images.githubusercontent.com/48195985/62111116-d9f03400-b2ea-11e9-8f55-e5eacf1c2ae9.png)버튼을 클릭합니다.

<br>

**2. 관리자 권한 부여하기**

> 가) “**기존 정책 직접 연결**”을 클릭하고 “**정책이름**”탭 아래 **AdministratorAccess**를 체크합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62111285-33586300-b2eb-11e9-8748-460d77370f21.png)

> 나) 창 아래 ![image](https://user-images.githubusercontent.com/48195985/62111382-63076b00-b2eb-11e9-8839-a6bee52eb931.png)를 클릭합니다.

<br>

**3. 태그 추가하기**

> 가) 키와 값에 다음 내용을 구성합니다.
> 
> ![image](https://user-images.githubusercontent.com/48195985/62111418-71558700-b2eb-11e9-928f-88f3d4993377.png)

> 나) 화면 하단의 ![image](https://user-images.githubusercontent.com/48195985/62111452-88947480-b2eb-11e9-8135-3d628acae6c7.png)를 클릭합니다.

<br>

**4. 사용자 추가**

> 가) 설정한 내용을 검토하고 하단의 ![image](https://user-images.githubusercontent.com/48195985/62111477-9cd87180-b2eb-11e9-9704-c71fcc8faf00.png)를 클릭합니다.

<br>

**5. 아래 표시된 부분의 URL 주소를 노트패드에 저장합니다.**

![image](https://user-images.githubusercontent.com/48195985/62111504-ae217e00-b2eb-11e9-9ade-2137ef679fc4.png)

_다음 주소와 유사합니다 : https://507815402614.signin.aws.amazon.com/console_

> 가) 화면 하단의 ![image](https://user-images.githubusercontent.com/48195985/62111581-ce513d00-b2eb-11e9-85d0-9e7f806d91f7.png)를 클릭합니다.


<br>

**6. 새로 생성한 IAM user 계정으로 로그인하기**

> 가) 웹 브라우저를 열고 복사해 놓은 URL을 입력합니다.\
> _다음 주소와 유사합니다 : https://507815402614.signin.aws.amazon.com/console_

> 나) 사용자이름 과 암호를 입력하고 로그인합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62111644-f640a080-b2eb-11e9-815e-5a7cb6fb6a97.png)


<br>

**7. Cloud9 환경 만들기**
> 가)  ![image](https://user-images.githubusercontent.com/48195985/62111734-31db6a80-b2ec-11e9-8f8a-ecef2c8646bb.png)메뉴에서 “Cloud9”을 클릭합니다.

> 나) ![image](https://user-images.githubusercontent.com/48195985/62111756-3f90f000-b2ec-11e9-9d5c-97675628fb2e.png)를 클릭합니다.

> 다) Name을 mzc-lab-ecs로 설정하고  ![image](https://user-images.githubusercontent.com/48195985/62111778-4b7cb200-b2ec-11e9-9c9f-fd2b0e4a87ee.png)
버튼을 클릭합니다.\
 ![image](https://user-images.githubusercontent.com/48195985/62111818-67805380-b2ec-11e9-9d40-a4a13a199288.png)

> 라) 환경구성 단계에서는 모두 기본값을 사용하고 ![image](https://user-images.githubusercontent.com/48195985/62111778-4b7cb200-b2ec-11e9-9c9f-fd2b0e4a87ee.png)을 클릭합니다.
 
> 마) 설정을 검토하고 화면 하단의 ![image](https://user-images.githubusercontent.com/48195985/62111756-3f90f000-b2ec-11e9-9d5c-97675628fb2e.png)을 클릭합니다.

> 바) Cloud9 도구가 시작되면 하단의 ①작업창과 ②시작탭을 닫습니다.\
![image](https://user-images.githubusercontent.com/48195985/62111915-95659800-b2ec-11e9-9055-91413d6a981f.png)

> 사) ③번의 ![image](https://user-images.githubusercontent.com/48195985/62111959-af06df80-b2ec-11e9-869d-60bfce079c40.png)기호를 클릭하고 “**New Terminal**”을 선택합니다.

> 아) 다음과 같이 작업공간이 구성됩니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62112000-c645cd00-b2ec-11e9-882c-4d24cad66dfc.png)

> 자) 테마를 변경하고 싶은 경우 **View ⇨ Themes ⇨ Solarized ⇨ Solarized Dark**를 선택하여 변경합니다.

<br>

**8. 도구 설치 및 구성하기**

> 가) ECS Cli 설치하기

```bash
sudo curl -so /usr/local/bin/ecs-cli https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-linux-amd64-latest

sudo chmod +x /usr/local/bin/ecs-cli

sudo yum -y install jq gettext
```

> 나) AWS Cli 명령을 적용할 Region을 현재 위치로 설정합니다.

```bash
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r .region)

echo "export AWS_REGION=${AWS_REGION}" >> ~/.bash_profile

aws configure set default.region ${AWS_REGION}

aws configure get default.region
```

<br>

**9. GitHub 계정 생성하기**

> 가) 웹 브라우저를 열고 https://github.com 사이트로 이동합니다.

> 나) 다음 창에 Username / Email / Password를 입력하고 “Sign up for GitHub”을 클릭합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62112273-584dd580-b2ed-11e9-8bcc-c772ad842c20.png)

> 다) Join GitHub 페이지에서 “**검증하기**” 버튼을 클릭하고 퍼즐을 풀어줍니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62112315-76b3d100-b2ed-11e9-8243-26bd077f4021.png)

> 라) 퍼즐을 풀고 나면 Welcome to GitHub 페이지로 이동합니다. 하단에 “**Continue**”버튼을 클릭합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62112509-cd210f80-b2ed-11e9-87e1-96a8f87d2ac3.png)

> 마) 몇가지 질문에 답합니다. 하단의 “**skip this step**”을 클릭하고 넘어가도 됩니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62112572-ec1fa180-b2ed-11e9-8420-18099dc8e334.png)

> 바) 이메일 확인을 요청하는 페이지가 나옵니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62112615-ffcb0800-b2ed-11e9-9185-2131a0dbcebb.png)

> 사) 등록하신 이메일로 이동해서 “Verify email address”를 클릭합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62112647-107b7e00-b2ee-11e9-9f18-e6c8ebdcf090.png)

> 아) GitHub 계정이 생성되었습니다.

<br>

**10. ssh 키를 생성해서 GitHub 계정에 추가합니다.**

> 가) ssh 키 생성하기

``` bash
mzclabadmin:~/environment $ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ec2-user/.ssh/id_rsa): <Enter>
Enter passphrase (empty for no passphrase): <Enter>
Enter same passphrase again: <Enter>
Your identification has been saved in /home/ec2-user/.ssh/id_rsa.
Your public key has been saved in /home/ec2-user/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:OUFSZ+klq5nJr4p+pJJUIAPVXc3xwoNZRVEiHCeYkes ec2-user@ip-172-31-24-192
The key's randomart image is:
+---[RSA 2048]----+
|o... ..o=OO**o.  |
|o . . .o+B*=..   |
| o .    +o++.    |
|    .   .ooo     |
|   .   oS=       |
|  .   . E.       |
| . . o   .       |
|  o ...   .      |
|   oo.....       |
+----[SHA256]-----+
mzclabadmin:~/environment $ 
```

> 나) 공개키를 클립보드로 복사하기

```bash
mzclabadmin:~/environment $ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRof4PxPjeIybx4muYqMtAnxRLf9bwwX4fu75lekhXqzQGAbJmRAiESRipj2kSN0zBtAL5kx0IIbtI4i3SgMaUkIY9MQBfXOZju2u93ZXvjVlr1CgomSKWhyRHIewPU/buEXXDe52J7YDppCDz56NTG1z7IVwL50xATlba2gLTI4jbxfy8NJRBA9xpeOtUvPV0hBdM3aTP9kTY4gWt+u1Tsrg7EVm8x7v0/+iuZ6lCOBZArIqY4yB8mA3DK/cM8As/v5RgvvdPru2H39hfGZxnzQX5ZFLJAS7QQzNuYoaN5hIt88Js3v+3gMObSzkMMMRHPdajsbgPGQRXPKgdD+pH ec2-user@ip-172-31-24-192
mzclabadmin:~/environment $ 
```

> 다) 다음주소를 클릭합니다. https://github.com/settings/ssh/new

> 라) 복사해 놓은 킷값을 Key 상자에 붙여넣고 “Add SSH Key” 버튼을 클릭합니다.(키는 _**ssh-rsa 부터 user@ip-address**_ 부분까지 복사합니다.)\
>![image](https://user-images.githubusercontent.com/48195985/62112902-94ce0100-b2ee-11e9-8610-44431e7c9d31.png)

> 마) 아래 그림과 유사하게 키가 추가됩니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62112972-b6c78380-b2ee-11e9-9a36-55027ba5e033.png)

<br>

**11. GitHub 토큰 생성하기**

> 가) 다음 주소를 클릭하고 다음 정보를 구성합니다.\
> https://github.com/settings/tokens/new

> * Note에 “mzc-lab-ecs”를 입력합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62113138-04dc8700-b2ef-11e9-9d7f-00097a50b40b.png)

> * Select Scopes 아래 repo를 체크합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62113176-132aa300-b2ef-11e9-9acf-fc938d680819.png)
> 
> * admin:repo_hook를 체크합니다.\
> ![image](https://user-images.githubusercontent.com/48195985/62113200-1e7dce80-b2ef-11e9-8140-4c64d2b57701.png)

> * 스크롤을 내려 하단의  ![image](https://user-images.githubusercontent.com/48195985/62113225-29d0fa00-b2ef-11e9-9809-0a61904d1080.png)버튼을 클릭합니다.
> 
>![image](https://user-images.githubusercontent.com/48195985/62113249-32c1cb80-b2ef-11e9-9d59-4a4e07c26272.png)

<br>

**12.  GitHub 토큰을 Cloud9 개발환경에 추가하기**
> 가) Cloud9 터미널에 다음 명령을 입력합니다. (your_github_token 은 생성하신 토큰으로 변경합니다.)
> ```bash
> export GITHUB_TOKEN=your_github_token
> ```

> 나) 설정을 저장하기
> ```bash
> echo "export GITHUB_TOKEN=${GITHUB_TOKEN}" >> ~/.bashrc
> ```

<br>

**13. 다음 GitHub repos를 자신의 GitHub account에 Fork 합니다.**
> * https://github.com/brentley/ecsdemo-platform/fork
> * https://github.com/brentley/ecsdemo-frontend/fork
> * https://github.com/brentley/ecsdemo-nodejs/fork
> * https://github.com/brentley/ecsdemo-crystal/fork


<br>

**14. GitHub에 로그인된 상태로 위 주소를 클릭하면 다음과 같은 페이지가 열립니다. 아래 여러분의 GitHub 계정을 클릭합니다.**

> ![image](https://user-images.githubusercontent.com/48195985/62113487-a9f75f80-b2ef-11e9-939b-1e0ae053b1e6.png)

> 가) 4번째 주소까지 추가한 후 상단의 여러분의 계정이름을 클릭합니다.
> 
> ![image](https://user-images.githubusercontent.com/48195985/62113525-bc719900-b2ef-11e9-853f-558d3fc87b97.png)


> 나) 4개의 주소를 모두 Fork 하면 다음처럼 여러분의 계정에 4개의 Repositories가 생성됩니다.
>
> ![image](https://user-images.githubusercontent.com/48195985/62113612-ddd28500-b2ef-11e9-8364-247f57257483.png)


<br>

**15. 이제 여러분의 작업공간에 Fork된 repos를 복제합니다.**

> 가) 다음 명령을 Cloud9 터미널에 입력합니다. (_**your_username**_ 값은 위에서 생성한 GitHub username 으로 변경합니다.)

> ```bash
> export YOUR_GITHUB_NAME=your_username
> export YOUR_GITHUB_NAME=itangma
> ```
> 
> 
> ```bash
> cd ~/environment
> git clone git@github.com:$YOUR_GITHUB_NAME/ecsdemo-platform.git
> ```
> 
> ```bash
> git clone git@github.com:$YOUR_GITHUB_NAME/ecsdemo-frontend.git
> git clone git@github.com:$YOUR_GITHUB_NAME/ecsdemo-nodejs.git
> git clone git@github.com:$YOUR_GITHUB_NAME/ecsdemo-crystal.git
> ```

**아래와 유사하게 출력됩니다.**

```bash
mzclabadmin:~/environment $ export YOUR_GITHUB_NAME=itangma
mzclabadmin:~/environment $ cd ~/environment
mzclabadmin:~/environment $ git clone git@github.com:$YOUR_GITHUB_NAME/ecsdemo-platform.git
Cloning into 'ecsdemo-platform'...
The authenticity of host 'github.com (192.30.255.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
RSA key fingerprint is MD5:16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.255.112' (RSA) to the list of known hosts.
remote: Enumerating objects: 94, done.
remote: Total 94 (delta 0), reused 0 (delta 0), pack-reused 94
Receiving objects: 100% (94/94), 19.69 KiB | 1.16 MiB/s, done.
Resolving deltas: 100% (37/37), done.
mzclabadmin:~/environment $ git clone git@github.com:$YOUR_GITHUB_NAME/ecsdemo-frontend.git
Cloning into 'ecsdemo-frontend'...
remote: Enumerating objects: 427, done.
Receiving objects: 100% (427/427), 98.29 KiB | 3.51 MiB/s, done.
remote: Total 427 (delta 0), reused 0 (delta 0), pack-reused 427
Resolving deltas: 100% (214/214), done.
mzclabadmin:~/environment $ git clone git@github.com:$YOUR_GITHUB_NAME/ecsdemo-nodejs.git
Cloning into 'ecsdemo-nodejs'...
remote: Enumerating objects: 155, done.
remote: Total 155 (delta 0), reused 0 (delta 0), pack-reused 155
Receiving objects: 100% (155/155), 33.46 KiB | 2.57 MiB/s, done.
Resolving deltas: 100% (75/75), done.
mzclabadmin:~/environment $ git clone git@github.com:$YOUR_GITHUB_NAME/ecsdemo-crystal.git
Cloning into 'ecsdemo-crystal'...
remote: Enumerating objects: 52, done.
remote: Counting objects: 100% (52/52), done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 149 (delta 25), reused 38 (delta 12), pack-reused 97
Receiving objects: 100% (149/149), 24.57 KiB | 3.07 MiB/s, done.
Resolving deltas: 100% (62/62), done.
mzclabadmin:~/environment $ 
```


<br>

**16. 여러분의 GitHub repos에서 Cloud9 개발환경으로 복제된 내용을 확인합니다.**
> Cloud9 터미널에서 다음 명령으로 복제된 repos를 확인할 수 있습니다.
> 
> ![image](https://user-images.githubusercontent.com/48195985/62113843-53d6ec00-b2f0-11e9-9bd8-4a6b420e145c.png)


<br>

## 작업 2 : 플랫폼 구축하기

기존 모놀리식 아키텍처를 마이크로서비스로 분해해서 앱 플랫폼을 관리합니다. 
이 워크샵에서는 repository를 시용하여 인프라를 관리하고 각 서비스가 개별 저장소에 유지 관리됩니다.
이 repository는 Acceptance와 Production이라는 2개의 독립적인 환경을 구축할 CloudFormation을 생성합니다.

**Diagram :**\
![image](https://user-images.githubusercontent.com/48195985/62113874-6b15d980-b2f0-11e9-9810-7694a1158153.png)


<br>

**17. AWS Cloud9 작업환경의 터미널에 다음 명령을 실행합니다.**

> 가) demo repository를 복제합니다.
> ```bash
> cd ~/environment
> git clone https://github.com/brentley/fargate-demo.git
> ```

> 나) 로드밸런서와 ECS 서비스에 대한 Role이 있는지 확인합니다.

> ```bash
> aws iam get-role --role-name "AWSServiceRoleForElasticLoadBalancing" || aws iam create-service-linked-role --aws-service-name "elasticloadbalancing.amazonaws.com"
> aws iam get-role --role-name "AWSServiceRoleForECS" || aws iam create-service-linked-role --aws-service-name "ecs.amazonaws.com"
> ```

> 다) VPC, ECS 클러스터 및 ALB 생성하기
> ```bash
> cd ~/environment/fargate-demo
> 
> aws cloudformation deploy --stack-name fargate-demo --template-file cluster-fargate-private-vpc.yml --capabilities CAPABILITY_IAM
> 
> aws cloudformation deploy --stack-name fargate-demo-alb --template-file alb-external.yml
> ```

> **CloudFormation을 이용해서 플랫폼 구성을 완료하였습니다.**

<br>

## 작업 3 : FrontEnd Rail App 배포하기

**18. 환경 변수 설정하기 (다음 명령을 Cloud99 터미널에 붙여넣습니다.)**

> ```bash
> export clustername=$(aws cloudformation describe-stacks --stack-name fargate-demo --query 'Stacks[0].Outputs[?OutputKey==`ClusterName`].OutputValue' --output text)
> export target_group_arn=$(aws cloudformation describe-stack-resources --stack-name fargate-demo-alb | jq -r '.[][] | select(.ResourceType=="AWS::ElasticLoadBalancingV2::TargetGroup").PhysicalResourceId')
> export vpc=$(aws cloudformation describe-stacks --stack-name fargate-demo --query 'Stacks[0].Outputs[?OutputKey==`VpcId`].OutputValue' --output text)
> export ecsTaskExecutionRole=$(aws cloudformation describe-stacks --stack-name fargate-demo --query 'Stacks[0].Outputs[?OutputKey==`ECSTaskExecutionRole`].OutputValue' --output text)
> export subnet_1=$(aws cloudformation describe-stacks --stack-name fargate-demo --query 'Stacks[0].Outputs[?OutputKey==`PrivateSubnetOne`].OutputValue' --output text)
> export subnet_2=$(aws cloudformation describe-stacks --stack-name fargate-demo --query 'Stacks[0].Outputs[?OutputKey==`PrivateSubnetTwo`].OutputValue' --output text)
> export subnet_3=$(aws cloudformation describe-stacks --stack-name fargate-demo --query 'Stacks[0].Outputs[?OutputKey==`PrivateSubnetThree`].OutputValue' --output text)
> export security_group=$(aws cloudformation describe-stacks --stack-name fargate-demo --query 'Stacks[0].Outputs[?OutputKey==`ContainerSecurityGroup`].OutputValue' --output text)
> cd ~/environment
> ```

<br>

**19. Cluster와 ecs-cli가 통신하도록 구성하기**

> ```bash
> ecs-cli configure --region $AWS_REGION --cluster $clustername --default-launch-type FARGATE --config-name fargate-demo
> ```
> 
> 명령을 실행할 때 참조할 기본 Region을 설정합니다.

<br>

**20. 외부에서 FrontEnd App과 통신하도록 TCP 3000번 포트를 허용하는 보안그룹 정책을 설정합니다.**

```json
aws ec2 authorize-security-group-ingress --group-id "$security_group" --protocol tcp --port 3000 --cidr 0.0.0.0/0
```

<br>

**21. 프론트엔드 애플리케이션 배포 :**

```bash
cd ~/environment/ecsdemo-frontend
envsubst < ecs-params.yml.template >ecs-params.yml

ecs-cli compose --project-name ecsdemo-frontend service up \
    --create-log-groups \
    --target-group-arn $target_group_arn \
    --private-dns-namespace service \
    --enable-service-discovery \
    --container-name ecsdemo-frontend \
    --container-port 3000 \
    --cluster-config fargate-demo \
    --vpc $vpc
```
> 여기서는 디렉터리를 Frontend 응용프로그램 코드 디렉토리로 변경합니다. envsubst 명령은 현재 값을 가진 ecs-params.yml 파일을 템플릿으로 합니다. 그런 다음 ECS 클러스터에서 Frontend 서비스를 시작합니다(Fargate의 기본 시작 유형 사용)

> 참고: ecs-cli는 서비스 검색을 위해 개인 dns 네임스페이스를 구축하고 cloudwatch 로그에 로그 그룹을 작성합니다.

<br>

**22. 실행중인 컨테이너 보기**

```bash
ecs-cli compose --project-name ecsdemo-frontend service ps \
    --cluster-config fargate-demo
```
> ![image](https://user-images.githubusercontent.com/48195985/62176399-49623400-b37c-11e9-8da0-562951d13eaf.png)
> 하나의 작업이 실행중입니다.

<br>

**23. ALB의 URL을 조회하여 출력하고 클릭해서 열거나 브라우저에 복사하여 붙여넣습니다.**

```bash
alb_url=$(aws cloudformation describe-stacks --stack-name fargate-demo-alb --query 'Stacks[0].Outputs[?OutputKey==`ExternalUrl`].OutputValue' --output text)

echo "Open $alb_url in your browser"
```

<br>

**24. 로그 확인하기**

```bash
#substitute your task id from the ps command 
ecs-cli logs --task-id de6cc928-c1c1-4386-904c-9ff2bc746646 \
    --follow --cluster-config fargate-demo
```
> task-id 는 22번에서 실행중인 컨테이너의 task-id로 변경합니다.

<br>

**25. 작업 규모를 조정합니다.**

```bash
ecs-cli compose --project-name ecsdemo-frontend service scale 3 \
    --cluster-config fargate-demo
ecs-cli compose --project-name ecsdemo-frontend service ps \
    --cluster-config fargate-demo
```
> 컨테이너를 3개의 가용영역에 균등하게 분배되도록 설정함.

<br>

**26. Frontend 앱서비스 페이지에서 분산처리 되는 화면을 볼 수 있습니다. (아래와 유사합니다.)**

![image](https://user-images.githubusercontent.com/48195985/62176421-5ed75e00-b37c-11e9-9eef-62fbad2f3bfe.png)


<br>

## 작업 4 : Node.js Backend API 배포하기

이 Backend API 서비스의 경우 API가 VPC 내부에서만 연결할 수 있도록 네이티브 서비스 검색을 사용합니다.

실행중인 컨테이너를 처리하기 위해 기본 서비스 검색을 사용하고 있습니다. http://ecsdemo-nodejs/로 가는 요청은 컨테이너가 시작할 때 등록하는 내부 route53 영역에 대한 dns SRV 조회를 발생시킵니다. 이는 프론트엔드 애플리케이션에서 처리됩니다. 프론트엔드 애플리케이션에서 이 코드 블록을 확인하십시오.\
https://github.com/brentley/ecsdemo-frontend/blob/master/app/controllers/application_controller.rb#L63

<br>

**27. Node.js 백엔드 응용프로그램 배포 :**

```bash
cd ~/environment/ecsdemo-nodejs
envsubst < ecs-params.yml.template >ecs-params.yml

ecs-cli compose --project-name ecsdemo-nodejs service up \
    --create-log-groups \
    --private-dns-namespace service \
    --enable-service-discovery \
    --cluster-config fargate-demo \
    --vpc $vpc
```
> 여기서는 디렉터리를 nodejs 응용 프로그램 코드 디렉토리로 변경합니다. envsubst 명령은 현재 값을 가진 ecs-params.yml 파일을 템플릿으로 합니다. 그런 다음 ECS 클러스터에서 nodejs 서비스를 시작합니다. (Fargate의 기본 시작 유형 사용)

>참고: ecs-cli는 서비스 검색을 위해 개인 dns 네임스페이스를 구축하고 cloudwatch 로그에 로그 그룹을 작성합니다.

<br>

**28. 실행중인 컨테이너 보기**

```bash
ecs-cli compose --project-name ecsdemo-nodejs service ps \
    --cluster-config fargate-demo
```
![image](https://user-images.githubusercontent.com/48195985/62176477-a958da80-b37c-11e9-9988-1488a84ccca0.png)


<br>

**29. 도달가능성 체크 (브라우저로 URL 열기)**

```bash
alb_url=$(aws cloudformation describe-stacks --stack-name fargate-demo-alb --query 'Stacks[0].Outputs[?OutputKey==`ExternalUrl`].OutputValue' --output text)

echo "Open $alb_url in your browser"
```
> 이명령은 ALB의 URL을 조회하여 출력합니다. 클릭하여 열거나 브라우저에 복사해서 붙여넣을 수 있습니다.

<br>

**30. 로그 보기 :**

```bash
#substitute your task id from the ps command 
ecs-cli logs --task-id e01941fe-ddae-4df2-9691-37d2d09ea86c \
    --follow --cluster-config fargate-demo
```
> task-id 는 28번에서 컨테이너의 task-id  값으로 변경합니다.

<br>

**31. 작업규모를 조정합니다.**

```bash
ecs-cli compose --project-name ecsdemo-nodejs service scale 3 \
    --cluster-config fargate-demo

ecs-cli compose --project-name ecsdemo-nodejs service ps \
    --cluster-config fargate-demo
```
> 이제 컨테이너가 3개의 가용영역에 분배되어 처리됩니다. \
![image](https://user-images.githubusercontent.com/48195985/62176523-d3120180-b37c-11e9-91d0-465d3b8f856a.png)

<br>

**32. 앱서비스 페이지에서 분산처리되는 화면을 볼 수 있습니다. (아래와 유사합니다.)**

![image](https://user-images.githubusercontent.com/48195985/62176540-e4f3a480-b37c-11e9-8072-4767deb67f82.png)

<br>


## 작업 5 : Crystal Backend API 배포하기

**33. Crystal 응용프로그램 배포하기**

```bash
cd ~/environment/ecsdemo-crystal
envsubst < ecs-params.yml.template >ecs-params.yml

ecs-cli compose --project-name ecsdemo-crystal service up \
    --create-log-groups \
    --private-dns-namespace service \
    --enable-service-discovery \
    --cluster-config fargate-demo \
    --vpc $vpc
```
> 여기서는 디렉터리를 Crystal 응용 프로그램 코드 디렉토리로 변경합니다. envsubst 명령은 현재 값을 가진 ecs-params.yml 파일을 템플릿으로 합니다. 그런 다음 ECS 클러스터에서 crystal 서비스를 시작합니다. (Fargate의 기본 시작 유형 사용)

> 참고: ecs-cli는 서비스 검색을 위해 개인 dns 네임스페이스를 구축하고 cloudwatch 로그에 로그 그룹을 작성합니다.

<br>

**34. 실행중인 컨테이너 보기**

```bash
ecs-cli compose --project-name ecsdemo-crystal service ps \
    --cluster-config fargate-demo
```
![image](https://user-images.githubusercontent.com/48195985/62176611-2be19a00-b37d-11e9-9c6b-7b07c29aa4b2.png)
> 동작중인 1개의 등록된 task가 보입니다.

<br>

**35. 도달가능성 체크 (브라우저로 URL 열기) - 기존에 열어놓은 페이지와 주소가 같습니다.** 

```bash
alb_url=$(aws cloudformation describe-stacks --stack-name fargate-demo-alb --query 'Stacks[0].Outputs[?OutputKey==`ExternalUrl`].OutputValue' --output text)

echo "Open $alb_url in your browser"
```
> 이 명령은 ALB의 URL을 조회하여 출력합니다. 클릭하여 열거나 브라우저에 복사해서 붙여넣을 수 있습니다.

<br>

**36. 로그 보기 :**

```bash
#substitute your task id from the ps command 
ecs-cli logs --task-id c9495f6e-d8ad-4f49-9c35-c0378eda04a3 \
    --follow --cluster-config fargate-demo
```
> task-id 는 34번에서 컨테이너의 task-id  값으로 변경합니다.

**37. 작업규모를 조정합니다.**

```bash
ecs-cli compose --project-name ecsdemo-crystal service scale 3 \
    --cluster-config fargate-demo

ecs-cli compose --project-name ecsdemo-crystal service ps \
    --cluster-config fargate-demo
```
![image](https://user-images.githubusercontent.com/48195985/62176715-81b64200-b37d-11e9-8afd-e22aef976494.png)

<br>

**38. 앱서비스 페이지에서 분산처리되는 화면을 볼 수 있습니다. (아래와 유사합니다.)**

![image](https://user-images.githubusercontent.com/48195985/62176761-9bf02000-b37d-11e9-86cb-0074a799b363.png)

<br>
<br>

## 작업 6 : 리소스 삭제하기

<span style="color:red">**실습 후 과금을 피하기 위해서 꼭 서비스를 삭제합니다.**</span>

<br>

**39. 컴퓨팅 리소스를 삭제합니다.**

```bash
cd ~/environment/ecsdemo-frontend

ecs-cli compose --project-name ecsdemo-crystal service rm --cluster-config fargate-demo
ecs-cli compose --project-name ecsdemo-nodejs service rm --cluster-config fargate-demo
ecs-cli compose --project-name ecsdemo-frontend service rm --delete-namespace --cluster-config fargate-demo

aws cloudformation delete-stack --stack-name fargate-demo-alb
aws cloudformation wait stack-delete-complete --stack-name fargate-demo-alb
aws cloudformation delete-stack --stack-name fargate-demo
```
<br>

**40. GitHub 정리하기**

* https://github.com/settings/tokens로 이동하여 mzc-lab-ecs 레이블이 붙은 토큰을 삭제하십시오.


* https://github.com/settings/ssh/로 이동하여 mzc-lab-ecs 레이블이 지정된 키를 삭제하십시오.

<br>

**41. Workspace를 정리합니다.**

> **Cloud9 EC2 인스턴스를 삭제합니다.**

> * 서비스 메뉴에 Cloud9을 클릭합니다. (아래와 같은 페이지가 열립니다.)
> * mzc-lab-ecs 작업환경이 선택되었는지 확인하고 우측 상단의  버튼을 클릭합니다.
> * 박스에 Delete를 입력하고  버튼을 클릭합니다.

<br>

## <span style="color:red">수고하셨습니다. 이제 Amazon ECS 실습을 완료하였습니다.</span>
