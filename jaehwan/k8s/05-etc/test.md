# Run Command HOL

---

**2020.07. MEGAZONE CLOUD. SA 정재환**

---

## Overview

---

AWS Systems Manager Run Command를 통해 관리형 인스턴스의 구성을 원격으로 안전하게 관리할 수 있습니다. 관리형 인스턴스는 모든 Amazon EC2 인스턴스이거나, Systems Manager용으로 구성된 하이브리드 환경의 온프레미스 머신입니다. Run Command를 사용하면 일반적인 관리 작업을 자동화하고 대규모로 애드혹 구성을 변경할 수 있습니다.

## 목표

---

Run Command HOL 진행으로 기능들을 배워보자~!
ㅁ RunCommand 실행 - Linux
ㅁ RunCommand 실행 - Windows
ㅁ Command 실행 결과 SNS 알림 통보
ㅁ Run Command용 Document 생성

## 개념 및 구성요소

---

## 장점

---

- **제어 및 보안:** IAM 정책과 규칙(Rule)을 사용하여 명령어와 인스턴스에 대한 접근 통제를 할 수 있습니다. 따라서 인스턴스에 직접 접근하는 사용자의 수를 줄일 수 있습니다.
- **신뢰성:** 구성 변경 템플릿을 생성하여 시스템의 신뢰성을 향상 시킬 수 있습니다. 이를 통해 예측 가능성을 높이면서 구성 불일치를 줄이기 위한 제어가 가능합니다.
- **가시성:** Run Command을 통해 명령어 추적을 지원하여 CloudTrail 와 통합을 통한 구성 변경 사항에 대한 시각화가 가능합니다.
- **용이성:** 미리 정의 된 명령어 세트를 선택하여 실행하고, 관리 콘솔, CLI 또는 API를 통해 진행 상황 추적을 할 수 있습니다.
- **사용자 정의:** Run Command를 자신의 조직의 요구에 맞춘 사용자 정의 명령어로 만들 수 있습니다.

## 활용 분야 및 사례

---

- 인스턴스에 대한 구성 변경을 일관성을 가지면서 임시(ad-hot) 구현할 필요성
- 여러 인스턴스에 걸쳐 신뢰성과 일관성있는 결과의 필요성
- 변경을 실시 할 수있는 사람과 할 수 있는 권한 제어의 필요성
- 어떤 조치가 이루어 졌는지의 명확한 감사(Auditing)의 경로
- 전체 원격 데스크톱 (RDP) 접근의 필요 없이 위의 사항을 하고자 하는 필요성

## 제약사항

---

## 가격정책

---

- Run Command는 무료로 제공됩니다.

## Architecture

---

## Session Manager 시작하기

---

## Task 1 : RunCommand 실행 - Linux Shell

> Managed Instances 들에 대해 실행명령을 특정 인스턴스 또는 다중 인스턴스에 동시에 명령을 수행합니다.

### Task 1.1 : Run Command 실행

1. 콘솔 메뉴에서 ->> **Systems Manger** 메뉴로 이동 ->> 왼쪽 탐색창에서 **Run Command** 선택합니다.
2. **`Run a Command`** 를 클릭 합니다.
3. **Command document** 화면 검색 창에서 Linux Instance에 shell 명령을 실행할 Document를 검색합니다.
4. 검색창을 클릭 한후 ->> **platform types** 을 클릭 후 ->> 연속적으로 **Linux** 를 클릭합니다.

    ![https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image1.png](https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image1.png)

5. 검색된 Document 들 중에서 **AWS-RunShellScript** 를 찾아 왼쪽 체크박스를 클릭하세요.
6. 또는 처음부터 검색창에 **AWS-RunShellScript** 를 직접 입력 하여 검색하여도 됩니다.

    ![https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image7.png](https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image7.png)

7. 검색된 Document의 왼쪽 체크박스를 클릭 한후 화면 아래 다음의 내용으로 구성합니다.
    - **Document version :** `1(Default)` 를 선택 합니다.
    - **Command parameters** 항목 아래
        - **Commands** 창에 다음의 script를 작성합니다.이 스크립트는 표준 출력으로 현재 시간을 출력 하도록 하고 **runcommand.txt** 라는 파일에 단순하게 *Run Command test* 라는 문자를 기록 하게 합니다.

            ```bash
            #!/bin/bash
            date 
            echo "Run Command test" >> runcommand.txt
            ```

        - **Working Directory :** `/tmp` 라고 입력 합니다. 이 의미는 Instance 의 OS 내에 명령이 실행될 위치를 선택 합니다.
    - **Targets** 항목 아래
        - **Choose instances manually** 를 선택한 후
        - **Instances** 아래 보여지는 모든 Linux instance들을 선택합니다.(왼쪽 체크박스 선택)

            ![https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image2.png](https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image2.png)

    - **Output options** 항목 아래 명령 실행 결과를 S3로 전달 합니다. (버킷 미리 생성)
        - **Enable writing to an S3 bucket** 왼쪽 체크박스에 체크가 되어 있는지 확인합니다.
        - **Choose a bucket name from the list** 를 체크하고 아래 콤보 박스에서 이전에 생성했던 S3버킷을 선택 합니다.
        - **S3 key prefix - optional** 항목에 `runcommand_output` 이라고 subfolder명을 입력 합니다.
    - **AWS command line interface command** 항목을 확장하면 **CLI command** 가 보입니다. 참고사항으로 이것은 현재 입력한 내용들을 바탕으로 CLI 명령이 완성된 것을 볼수 있습니다.
8. 화면 아래 **Run** 을 클릭 하여 run command를 실행합니다.
9. 실행중인 화면이 보입니다. 새로고침 아이콘 을 클릭하면서 정상적으로 실행이 완료 될 때까지 잠시 기다립니다. 현재 실습에서는 모든 instance가 정상적으로 실행되지 않아도 됩니다.

    ![https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image3.png](https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image3.png)

---

### Task 1.2 : Run Command 실행 결과 확인

10. 콘솔 화면 왼쪽 탐색창에서  **`Run Command`**를 선택 합니다.

11. 오른쪽 화면에 **Commands** 탭을 선택 합니다.

12. **Commands** 화면 아래 **Command ID** 필드에 현재 실행 중인 또는 실행 완료된 ID가 보이면 클릭 하세요.

13. **Targets and outputs** 화면 아래 실행 완료된 또는 실행 중인 인스턴스들의 **Status** 필드에서 **Sucess** 표시가 된 Instance ID 왼쪽에 체크 합니다. 주:) Instance ID를 선택 하지 마세요.

- 선택후 오른쪽에 **`View output`** 버튼을 클릭하여 합니다.
- Output 화면으로 바뀐후 **Step 1 - Output** 오른쪽 `s3` 아이콘을 클릭 하여 S3콘솔로 이동합니다.

    ![https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image4.png](https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image4.png)

- 화면에 보이는 폴더를 클릭 합니다. **0.awsrunShellScript** 이와 같은 이름이거나 비슷한 형식일 것입니다.
- stdout 객체가 보이고 이 객체를 선택하여 다운로드 한 후 txt 파일 편집기에서 내용을 확인해 보세요. 대략 다음과 같은 내용이 보이실 겁니다.

```bash
Thu May 16 03:06:55 UTC 2019
```

이 출력은 표준 출력의 값이 객체에 저장된 내용으로 위에서 작성한 스크립트가 실행시 실행과정에서 발생하는 표준출력(화면출력)의 내용이 저장된 것을 알 수 있습니다.

---

### Task 1.3 : Session manager로 성공한 인스턴스에 접속

> 각 인스턴스내에 /tmp 디렉터리에서 shell script를 실행하도록 되어 있습니다. 정상적인 실행이 되었는지 `Run Command`가 성공한 인스턴스에 접속하여 확인해 봅니다.

14. 콘솔 메뉴 ->> **Systems Manger** 메뉴 ->> 왼쪽 탐색창 **Run Command** 를 선택 후 보여지는 ->> **Commands** 탭을 선택 합니다.

15. **Command ID** 필드에 현재 실행 중인 또는 실행 완료된 ID가 보이면 클릭 하세요.

16. **Targets and outputs** 화면 아래 실행 완료된 또는 실행 중인 인스턴스들의 **Status** 필드에서 **Sucess** 표시가 된 **Instance ID** 를 기록해 둡니다.

17. 왼쪽 탐색창 **Session Manager** 를 선택한 후 **Start session**을 클릭 합니다.

- **Target instances** 아래에서 방금 기록해둔 Instance ID를 찾아 선택 한 후 **Start session**을 클릭 합니다.

18. 새로운 탭에 Session창이 보이며 이곳에 다음의 명령을 수행하고 아래의 결과와 비슷하게 출력이 되는지 확인해 봅니다.

```bash
$ ls -l /tmp/runcommand.txt

-rw-r--r-- 1 root root 17 May 16 03:06 /tmp/runcommand.txt
```

```bash
$ cat /tmp/runcommand.txt

Run Command test
```

파일이 생성된 것을 확인 할 수 있습니다. 이 파일은 Run Command의 의해서 작성한 스크립트가 Instance 내에서 동작이 되어서 생성된 것입니다.

**`Terminate`**를 클릭 하여 Session을 중단합니다.

---

### Task 1.4 : Pending된 Command 중단 (옵션 작업)

> Command가 정상적으로 완료되지 못하고 pending 상태가 된 경우에만 다음과 작업을 진행합니다.

19. 콘솔 메뉴 ->> **Systems Manger** 메뉴 ->> 왼쪽 탐색창 **Run Command** 를 선택 후 보여지는 ->> **Commands** 탭을 선택 합니다.

20. **Status** 필드에 In Progress 상태가 있는 Command ID를 확인 하고 왼쪽에 선택 버튼에 체크 합니다. 주:) Command ID를 클릭 하지 마세요.

21. 그런다음 **Cancel command** 를 클릭 합니다. **Cancel command** 확인 창이 보이면 **cancel command** 를 클릭 합니다.

22. 이제 Pending 된 Command가 중단 되었으며 **Command history** 탭을 클릭 하면 **Status** 필드에 cancelled 가 된 Command 가 보입니다.

## Task 2 : RunCommand 실행 - Windows

> Windows 운영체제에 Inventory 목록을 만들기 위한 Command를 실행합니다. 즉 Instance 내의 Window 운영체제 명령어들을 실행하는 작업으로 Windows 운영체제의 기본 정보(버전)등을 목록화 하는 작업입니다. 이 작업은 시스템에 미리 구성되어 있는 Document를 사용합니다.

---

### Task 2.1 : Run Command 실행

1. 왼쪽 탐색창에서 **Run Command** 선택합니다.
2. **`Run a Command`** 를 클릭 합니다.
3. **Command document** 화면 검색창에서 실행할 Document를 검색합니다.
4. 검색창을 클릭 후 ->> `AWS-ListWindowsInventory` 를 입력 한후 엔터키를 입력 합니다.
    - **Targets** 항목 아래
        - **Choose instances manually** 를 선택 한후
        - **Instances** 아래 보여지는 한개의 Windows instance (`SSMWindowsInstance`) 를 선택합니다. (왼쪽 체크박스 선택)
    - **Output options** (S3로 실행된 결과를 전달 하기 위해 아래 항목을 선택 합니다.
        - **Enable writing to an S3 bucket** 왼쪽 체크박스에 체크가 되어 있는지 확인합니다.
        - **Choose a bucket name from the list** 를 체크하고 아래 콤보 박스에서 실습에서 생성했던 S3버킷(`ssm-test-xxx`) 를 선택 합니다.
        - **S3 key prefix - optional** 항목에 `runcommand_output` 이라는 subfolder명을 입력 합니다.
    - **AWS command line interface command** 항목을 확장하면 **CLI command** 가 보입니다. (참고사항으로 이것은 현재 입력한 내용들을 바탕으로 CLI 명령으로 수행할 수 있도록 제공되는 기능입니다.)
    - 화면 아래 **Run** 을 클릭 하여 run command를 실행합니다.
    - 실행 중인 화면이 보입니다. 새로고침 아이콘을 클릭하면서 정상적으로 실행이 완료 될 때까지 잠시 기다립니다.
    - 잠시 후 **Status** 필드에 Success 가 보이면 정상적으로 수행이 완료 된 것입니다.

---

### Task 2.2 : Run Command 실행 결과 확인

6. 현재 화면에서 **Targets and outputs** 화면 아래 실행 완료된 인스턴스의 **Status** 필드에서 **Sucess** 표시가 된 Instance ID 왼쪽에 체크 합니다. 주:) Instance ID를 클릭 하지 마세요.

- 선택후 오른쪽에 **View output** 버튼을 클릭하여 합니다.
- **Output** 화면으로 바뀐후 **Step 1 - Output** 을 확장하면 다음과 비슷한 실행 결과 화면이 보입니다.

```powershell
  The command output displays a maximum of 2500 characters. You can view the complete command output in either Amazon S3 or CloudWatch logs, if you specify an S3 bucket or a CloudWatch logs group when you run the command.

  Get-OSInventory - EC2AMAZ-6T4QI6Q - 2019-05-16T07:33:41Z
  Schema: 1.0
  Publisher: Microsoft Corporation
  OSName: Microsoft Windows Server 2019 Datacenter
  OSVersion: 10.0.17763
  ExtendedProperties:
  Key: ServicePackLevel
  Value: null
  Key: OSArchitecture
  Value: 64-bit
  Key: OSLanguageId
  Value: 1033
  Key: OSLanguage
  Value: English (United States)
  Key: ProductType
  Value: 3
  Key: OperatingSystemSKU
  Value: 8
  Key: OperatingSystemSKUName
  Value: Datacenter Server Edition 
```

## Task 3 : Command 실행 결과를 SNS 알림으로 통보

> Run Command로 실행된 결과에 대해 SNS알림으로 통보를 받기위해 다음의 설정을 진행합니다. 이는 SSM 서비스가 SNS 서비스를 사용해야 하므로 반드시 SSM에게는 SNS 서비스를 사용해야할 권한이 필요합니다. 이 권한은 Role을 통해 설정을 하게 됩니다.

---

### Task 3.1 : SNS 알림 사용을 위한 IAM 역할 생성

> SSM 서비스에게 SNS 서비스를 사용할 수 있는 권한을 부여하기 위해 Role을 생성합니다.

1. **Services** 에서 ->> **IAM** 서비스 선택 ->> 왼쪽 탐색창 **Roles** 선택

2. **Create role** 버튼을 클릭 후에 다음을 진행합니다.

- **Create role** 화면에서 ->> *Select type of trusted entity* 아래 `AWS 서비스` 상자를 선택한 후
- 이어서 *Choose the service that will use this role* 내용 아래 **`EC2`**를 선택 합니다.
    - 화면 아래 **Next: Permissions** 버튼을 클릭합니다.
    - **Attach permissions policies** 화면아래 검색창에 `AmazonSNSFullAccess` 를 입력하면 **AmazonSNSFullAccess** 가 검색되며 왼쪽 체크박스에 체크하여 선택함.
    (주의:검색된 policy 이름을 클릭 하지 마세요.)
    - 화면 아래 **Next: Tags** 를 클릭합니다.
    - **Add tags** 화면에서 tag를 입력합니다. 이 과정은 Skip해도 됩니다.
        - **Key** : `project`
        - **Value** : `ssmtest`
    - 화면 아래 **Next: Review**를 클릭 합니다.
    - Review 화면에서 다음의 내용을 입력합니다.
        - **Role name*** : `SSM_with_SNS_Role`
    - 화면 아래 **Create role**을 클릭 하여 Role생성을 완료합니다.

3. Role 생성 후 검색 창에 `SSM_with_SNS_Role`을 검색합니다.

4. 검색된 Role이름을 클릭 합니다. **Trust relationship** 에 SSM를 추가하려고 합니다.

- **Summary** 화면 아래 ->> **Trust relationships** 탭을 클릭한 후 **Edit trust relationship** 을 클릭 합니다.
- **Policy Document** 창이 보이면 다음과 같이 "ssm.amazonaws.com" 을 추가 합니다. 다음과 같이 수정하세요.

```json
  {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": ["ec2.amazonaws.com","ssm.amazonaws.com"]
      },
      "Action": "sts:AssumeRole"
    }
   ]
  }
```

- 화면아래 **Update Trust Policy** 를 클릭 하여 수정합니다. 그럼 **Summary** 페이지에 다음 그림처럼 **Trusted entities** 에 SSM과 EC2가 추가 되어 있는 것을 볼 수 있습니다.

![https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image5.png](https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image5.png)

- 계속하여 현재 수정중인 **Summary** 페이지 아래에 **Role ARN** 값을 메모장에 복사해 놓습니다.
    - **Role ARN :** `______________________`예를 들어 다음과 비슷합니다. 
    (ex> arn:aws:iam::1122334455:role/SSM_with_SNS_Role)
- Permissions 탭을 클릭 합니다. *iam:PassRole* 정책을 추가 하려고 합니다.
    - **Permissions policies** 항목 옆에 `Add inline policy` 링크를 클릭 합니다.
    - JSON 탭을 클릭 하고 아래와 같이 내용을 입력합니다. 이때 **Resource** 옆 ARN번호는 방금전 복사한 **Role ARN** 번호를 사용합니다.

        ```json
        {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Sid": "VisualEditor0",
                  "Effect": "Allow",
                  "Action": "iam:PassRole",
                  "Resource": "[이곳에 ARN 번호를 붙여넣으세요.]"
              }
          ]
        }
        ```

        예를 들어 다음과 비슷하게 작성하시면 됩니다.

        ```json
        {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Sid": "VisualEditor0",
                  "Effect": "Allow",
                  "Action": "iam:PassRole",
                  "Resource": "arn:aws:iam::1122334455:role/SSM_with_SNS_Role"
              }
          ]
        }
        ```

    - **Review policy** 를 클릭 합니다.
    - 이어서 Review policy 화면에서 문제가 없는지 검토후 **Name*** 필드에 `SSM_with_SNS_passRole_policy` 이라고 이름을 입력한 뒤 **Create policy** 를 클릭 하여 권한 추가를 완성 합니다.

---

### Task 3.2 : Amazon SNS Topic 생성

5. SNS 서비스 메뉴로 이동 ->> 왼쪽 탐색창에서 **Topics**을 클릭 합니다.

6. **`Create topic`** 을 클릭후 다음 화면에서 다음의 값을 입력 합니다.

- **Name :** `SSM_event_topic`
- **Display name - optional :** `SSM_event_topic`
- **Create topic** 을 클릭하여 Topic생성을 완료 합니다.
- 이때 정상적으로 생성이 완료가 되었다는 메세지가 보이며 생성된 Topic 화면으로 바뀝니다. 화면상에서 ARN 번호를 확인 하고 메모장에 복사합니다.
    - **ARN :** `__________________________________________`예를 들어 다음과 비슷합니다. 
    ( ex> arn:aws:sns:us-west-2:123456789:SSM_event_topic )

7. 계속해서 구독을 생성합니다. 왼쪽 탐색창에서 **Subscriptions** 를 선택한 후에 화면에서 **Create subscription** 을 클릭 합니다.

- **Details** 항목 아래에 **Topic ARN** 아래 검색창을 클릭 해서 방금 전에 생성한 Topic (`SSM_event_topic`)을 선택 합니다.
- **Protocol** 아래 콤보박스를 클릭 하여 `Email` 을 선택 합니다.
- **Endpoint** 에 확인 할 실수 있는 Email 주소를 입력 합니다.
- **Create subscription** 을 클릭 합니다.

8. 잠시 후 본인의 메일함에 `AWS Notification - Subscription Confirmation` 이란 제목으로 신규 메세지가 왔는지 확인해 주십시요. 메일이 오지 않았다면 스팸 메일함도 꼭 확인해 주세요.

9. 메일 내용 중에  `Confirm subscription`을 클릭 하여 Confirm을 완료해 주세요.

10. 콘솔 화면으로 돌아와서 왼쪽 탐색창에 **Subscriptions** 를 선택 후 생성한 Subscription의 **Status** 필드에 Confirmed 라고 보이는지 확인합니다. 정상적으로 메일을 수신하는 것에 동의했다는 의미입니다.

![https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image6.png](https://jk-sumerian.s3.ap-northeast-2.amazonaws.com/SystemsManager/Version1_0_0/images/03.RunCommand/image6.png)

---

### Task 3.3 : Run Command를 실행하면서 SNS 알림을 설정함.

11. **Systems Manager** 서비스 화면으로 이동 ->> 탐색창에서 **Run Command** 선택

12. **`Run a Command`** 를 클릭 합니다.

13. **Command document** 화면 검색창에서 Linux shell 을 실행할 Document를 검색합니다.

14. 검색창을 클릭 후 ->> **AWS-RunShellScript** 입력 하여 찾은 Document의 옆 선택 상자를 클릭합니다. (* 주의: Document 이름을 클릭 하지는 마세요)

15. **Command parameters** 항목 아래 다음 내용으로 구성합니다.

- **Commands** 창에 다음의 script를 작성합니다.

    ```bash
    #!/bin/bash
    date
    echo "Hi"
    echo "Run Command test" >> runcommand.txt
    ```

- **Working Directory :** `/tmp` 라고 입력 합니다. OS 내에 명령 실행 위치를 선택 합니다.
- **Targets** 항목 아래
    - **Choose instances manually** 를 선택 한후
    - **Instances** 아래에 한개의 Linux instance (`SSMLinuxInstance`)를 선택합니다.
- **Output options** 항목 아래 (명령 실행 결과를 S3로 전달 합니다. 버킷은 이전 실습에서 생성한 S3 버킷을 사용합니다.)
    - **Enable writing to an S3 bucket** 왼쪽 체크박스에 체크가 되어 있는지 확인합니다.
    - **Choose a bucket name from the list** 를 체크하고 아래 콤보 박스에서 이전에 생성했던 S3버킷(`ssm-test-xxx`) 를 선택 합니다.
    - **S3 key prefix - optional** 항목에 `runcommand_output` 이라고 Prefix(subfolder)명을 입력 합니다.
    - **SNS notifications** 항목을 확장합니다.
        - **Enable SNS notifications** 항목 왼쪽 체크박스에 체크합니다.
        - **IAM role :** `SSM_with_SNS_Role` 선택
        - **SNS topic :** 이전에 메모장에 복사했던 **Topic ARN** 을 입력 합니다.
        - **Notify me on** 에서 `Success` 를 선택하여 Command가 성공했을때만 메세지를 받도록 합니다. 기본적으로 선택되어 있는 `All events` 가 보이면 'x'를 눌러 제거 하세요.
- **AWS command line interface command** 항목을 확장하면 **CLI command** 가 보입니다. 이부분은 참고사항으로 현재 입력한 내용들을 바탕으로 CLI 명령이 완성된 것을 볼수 있습니다.
- 화면 아래 **`Run`** 을 클릭 하여 run command를 실행합니다.

16. 실행 중인 화면이 보입니다. 새로고침 아이콘을 클릭하면서 정상적으로 실행이 완료 될 때까지 잠시 기다립니다.

17. 잠시 후  당신의 Email함에 메일이 도착 한것을 확인해 보세요.

## Task 4 : Run Command용 Document 생성

> 기존에 정의 되어 있던 Document를 사용하지 않고 새롭게 Document를 생성해보는 실습을 진행합니다. Linux용으로 작성됩니다.

### Task 4.1 : Web서버 설치용 Document 생성

> 다음의 실습 예는 Linux 인스턴스에 WEB 서버를 설치하는 Document를 작성하고 RunCommand에서 실행하는 내용 입니다.

1. **Systems Manager** 서비스 화면으로 이동 ->> 왼쪽 탐색창에서 **Documents** 선택
2. **Create command or session** 버튼을 클릭후 **Create document** 화면에서 다음의 내용을 입력 합니다.
    - **Name :** `SSM_Installation_WEB`
    - **Target type - optional :** `/AWS::EC2::Instance`
    - **Document type - optional :** `Command document`
    - **YAML** 선택 후 아래 내용을 기입

        ```yaml
        ---
        schemaVersion: '2.2'
        description: Installation a WEB server
        parameters: {}
        mainSteps:
        - action: aws:runShellScript
          name: configureServer
          inputs:
            runCommand:
            - sudo yum install -y httpd
            - sudo echo "<p><strong>Hello, world!</strong></p>" > /var/www/html/index.html
            - sudo service httpd start
        ```

    WEB 서버를 설치 하는 스크립트를 작성합니다. 이 문서의 내용은 httpd 데몬을 설치 하고 간단한 html 문서를 생성하는 linux 명령으로 조합되어 있습니다.

    - **`Create document`** 버튼을 클릭 하여 Document 생성을 완료 합니다. 생성된 Document 확인은 Owned by me Tab을 확인
3. 탐색창 왼쪽에 **Run Command** 를 선택 후 **Run Command** 를 클릭 합니다.
4. **Command document** 화면 검색 창에서 Linux shell을 실행할 Document를 검색합니다.
5. 검색창을 클릭 한 후 ->> **SSM_Installation_WEB** 입력 후 엔터를 치면 Document가 검색됩니다. 이때 화면에 표시된 SSM_Installation_WEB Document 에서 왼쪽 선택 버튼을 클릭한 후에 아래같이 내용을 입력 합니다.
    - **Targets** 항목 아래
        - **Choose instances manually** 를 선택 하신후
        - **Instances** 아래에 (`SSMLinuxInstance`)를 선택합니다.
    - **Output options**
        - **Enable writing to an S3 bucket** 왼쪽 체크박스에 체크가 되어 있는지 확인합니다.
        - **Choose a bucket name from the list** 를 체크하고 아래 콤보 박스에서 이전에 생성했던 S3버킷(`ssm-test-xxx`) 를 선택 합니다.
        - **S3 key prefix - optional** 항목에 `runcommand_output` 이라고 Prefix(subfolder) 명을 입력 합니다.
        - 화면 아래 **Run** 을 클릭 하여 run command를 실행합니다.
6. 실행중인 화면이 보입니다. 새로고침 아이콘 클릭하면서 정상적으로 실행이 완료 될 때까지 잠시 기다립니다.
7. 정상적으로 완료가 되었으면 session manager를 통해 *SSMLinuxInstance* 인스턴스에 session 연결한 후 다음의 명령을 실행하여 http 프로세스가 정상적으로 동작 하는지 확인 합니다.

```bash
$ ps -ef | grep httpd 
root      3307     1  0 08:55 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    3308  3307  0 08:55 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    3309  3307  0 08:55 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    3310  3307  0 08:55 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    3311  3307  0 08:55 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    3312  3307  0 08:55 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
```

---

### Task 4.2 : 설치된 Web 서버의 보안 그룹 설정

> Brower 접근을 위해 인스턴스의 보안그룹(Security group)에 HTTP Inbound를 설정합니다. 현재 실습 환경에서는 해당 인스턴스 보안그룹의 Inbound 설정은 모두 차단 되어 있어 WEB page 접속을 위해 HTTP Inbound rule 설정을 진행합니다.

8. **EC2** 서비스 화면이동 ->> 왼쪽 탐색창에 **Security Groups** 선택 ->> **Create Security Group** 클릭한 후 다음 정보를 입력 합니다.

- **Security group name :** `SSM_WEB_SG`
- **Description :** `Allow HTTP Protocol to SSM`
- **VPC :** `SSM_test_VPC`
- Inbound Tab 아래 **Add Rule** 버튼 클릭
- **Type :** `HTTP` 선택
- **Source :** `Anywhere` 선택
- 화면아래 **Create** 를 클릭 하여 보안그룹 생성을 완료 합니다.

9. 생성된 보안 그룹을 인스턴스에 적용하기 위해 왼쪽 탐색창에서 ->> **Instances** 를 선택 합니다.

10. `SSMLinuxInstance` 인스턴스 이름 위에 마우스 오른쪽 버튼을 클릭 한후 ->> **Networking** ->> **Change Security Groups** 를 선택 합니다.

- **Change Security Groups** 화면에서 기존에 사용중인 `SSM_Only_SG` 왼쪽 체크박스에서 선택을 해제 합니다.
- `SSM_WEB_SG` 이름을 찾아 왼쪽 체크박스에 체크를 합니다.
- **Assign Security Groups** 를 클릭 하여 보안 그룹 변경을 완료합니다.

11. `SSMLinuxInstance` 가 선택 되어 있는 화면 아래 **Public IP**를 복사 하여 새로운 Brower 에 입력 하여 WEB 화면에 Hello, world! 가 표시 되는지 확인 합니다.

---

## 실습 끝!!!
