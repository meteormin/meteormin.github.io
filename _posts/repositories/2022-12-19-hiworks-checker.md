---
title: Hiworks Check in and out
excerpt: My Python Repositories
categories: repositories
tags: python
---

# automatic hiworks checkin and out

## Purpose & Reason

회사의 근태 관리 정책이 불합리 하다고 생각하여 개인적으로 개발을 진행 했다.

야근 식대는 계약할 때 초과 2시간 이상 근무일 때 신청할 수 있는 것으로 안내 받았음. 그러나 야근을 미리 보고 하여야 인정 해줌
대체 누가 "오늘은 야근 해야 겠다!" 하고 야근을 하는가? 이런 식의 정책은 결국 야근을 하게 되더라도 야근 식대를 최대한 줄이기 위한 조치로 밖에 보이지 않는다.

그렇다면, 야근을 하게 되었을 경우 다음날은 조금 일찍 퇴근해도 할 말 없지 않은가?
위에서 설명한 정책으로 바뀌기 이전에는 초과 근무에 대한 내용이 하이웍스에 기록이 되었다. 하지만 이제는 초과 근무로 퇴근을 찍어도 무조건 8시간으로 기록이 된다.
그렇다면 야근을 해도 기록상으로 나는 항상 정시 퇴근한 것으로 통계에 나타난다. 하지만 1분이라도 조기 퇴근을 하게 되면 "조기 퇴근"이라고 표시된다.
그래도 보고 하면 야근 식대 주긴 준다! 라는 보여 주기 식 정책에 기분이 좋지 않았기 때문에 근태 자동화 및 스케줄링 기능과 출퇴근 내역의 히스토리를 남길 수 있는 수단이 필요했다.

## Install

### requirements

- python ^3.10 (하위 버전 호환 X)
- pip ^22.2.2

**Packages**

```requirements.txt
click~=8.1.3
selenium==4.6.0
webdriver-manager~=3.8.4
pytimekr~=0.1.0
packaging~=21.3
pandas~=1.5.1
APScheduler~=3.9.1.post1
git+https://github.com/meteormin/dfquery.git
```

### install

**Install Python**

Install python3.10: https://www.python.org/downloads/

For Mac homebrew: https://formulae.brew.sh/formula/python@3.10

```shell
homebrew install python@3.10
```

**Download source code**

```shell
git clone https://github.com/smyoo-testworks/hiworks-check-in-and-out.git

python -m venv venv

# Mac
source ./venv/activate

# Linux
./venv/activate

# Windows(CMD)
.\venv\activate

pip3 install -r requirements.txt
```

## Usage

### Configure

settings.ini

```ini
[hiworks]
url = https://office.hiworks.com/testworks.onhiworks.com/
id = your-id
password = your-password

[mailer.outlook]
url = outlook.office365.com
id = your-id
password = your-password
```

### Python CLI

```shell
python3 cli.py {command-name} {arguments} {--options}

python3 cli.py checkin --id={your-hiworks-id} --passwd={your-hiworks-password}

python3 cli.py checkout --id={your-hiworks-id} --passwd={your-hiworks-password}
```

### Shell Script

```shell
# help about commands
./hiworks-checker.sh --help

# checkin and checkout
sh ./hiworks-checker.sh {checkin or checkout}
# or
./hiworks-checker.sh {checkin or checkout}

# check work hour
./hiworks-checker.sh check-work-hour

# check and alert
./hiworks-checker.sh check-and-alert

# Report For Month
./hiworks-checker.sh repotrt-for-month
```

### Scheduler

**use Crontab**

```shell
# 09시 정각 checkin
00 09 * * 1-5 cd /{your-path}/hiworks-check-in-and-out && sh hiworks-checker.sh checkin >> /{your-path}/hiworks-check-in-and-out/cron.log 2>&1

# 18시 정각 checkout
00 18 * * 1-5 cd /{your-path}/hiworks-check-in-and-out && sh hiworks-checker.sh checkout >> /{your-path}/hiworks-check-in-and-out/cron.log 2>&1

# 08 ~ 22시까지 10분마다 실행
*/10 08-22 * * 1-5 cd /{your-path}/hiworks-check-in-and-out && sh hiworks-checker.sh check-and-alert >> /{your-path}/hiworks-check-in-and-out/cron.log 2>&1

```

**schedule command**
> schedule 커맨드를 통해 크론 없이 스케줄링이 가능합니다.

> 상대적으로 불안정하여, 하루에 한번 재시작 권장 

```shell

hiworks-checker.sh schedule

# Windows
hiworks-checker.bat schedule

```

schedule configuration
> [scheduler.py](https://github.com/meteormin/hiworks-check-in-and-out/blob/main/config/scheduler.py)

```python
# python 딕셔너리 작성

SCHEDULER = {
  "test": {
    "func": "test(command-name)",
    "args": [
      "argument1",
      "argument2"
    ],
    "hour": "*",
    "minute": "*",
    "second": "10",
    "day_of_week": "*"
  }
}
```

- func: CLI 명령에 등록 되어 있는 명령어 이름을 전달 합니다.
    - 단, '-'이 포함된 명령은 '-' 대신 '_'(으)로 표기 해야 합니다.
- args: CLI 명령에 등록 되어 있는 명령어의 옵션 및 인수를 전달 합니다.
- hour: 등록할 주기의 시간 입니다.
- minute: 등록할 주기의 분 입니다.
- second: 등록할 주기의 초 입니다.
- day_of_week: 등록할 주기의 요일 입니다.
- hour, minute, second, day_of_week은 crontab의 표현식을 사용합니다.
    - 좀 더 자세한 표현식 참조
        - Cron 위키 피디아: https://en.wikipedia.org/wiki/Cron#CRON_expression
        - APScheduler 공식 문서: https://apscheduler.readthedocs.io/en/3.x/modules/triggers/cron.html


### [Hiworks Check in and out](https://github.com/meteormin/hiworks-check-in-and-out)
[![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=meteormin&repo=hiworks-check-in-and-out&show_owner=true&theme=nord)](https://github.com/meteormin/hiworks-check-in-and-out)
