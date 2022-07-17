---
title: Mapper
excerpt: My PHP Repositories
categories: php
---

## Tistory API for Laravel

> Version: Beta-0.0.1

라라벨용 티스토리 API입니다.<br>
티스토리 API는 OAuth2.0인증을 사용하지만 아쉽게도 API Key를 이용하여 인증 토큰을 얻지 않고 Authorize 권한인증을 통해 Redirect된 웹 화면에서 별도의 로그인 과정을 거쳐야 access
token을 발급 받을 수 있습니다.<br>
로그인 과정을 웹페이지에서 진행하여야 하기 때문에 백그라운드에서 API를 사용하려면 웹 크롤러의 사용이 필수입니다.<br>
이 패키지는 PHP 개발환경(Laravel)에서도 자동으로 티스토리에 포스팅해보고자 웹 크롤러 및 라라벨 프레임워크를 활용하여 자동 로그인과 자동 포스팅을 구현했습니다.

## 요구사항

- 기본적으로 PHP, Composer, Laravel(혹은 Lumen) 프레임워크 등의 설치가 필요합니다.
    - PHP 7.4 버전 이상
    - Laravel 7 ~ 8 버전
    - composer 2.0 이상
- 이 패키지는 [php-webdriver/php-webdriver](https://github.com/php-webdriver/php-webdriver) 패키지를 사용합니다.
    - Google Chrome 브라우저가 설치되어 있어야 합니다.
    - Google Chrome 버전에 맞는 Chrome Driver를 설치해야 합니다.
- Linux의 경우, 해당 패키지에 artisan console 명령어로 Chrome Driver를 실행 시키는 명령이 존재합니다.
    - 리눅스 Screen 패키지가 필요합니다.
    - 라라벨 스케줄링 기능을 활용하여, 원하는 시간에 자동으로 실행할 수 있게 설정하시면 됩니다.

- Windows의 경우, Chome Driver를 수동으로 실행 시키거나, 스케줄러 등록을 하시면 됩니다.
    - 추후 Windows도 Chrome Driver를 실행 시킬 수 있는 Artisan 명령어도 구현할 예정입니다.

### 주의사항

Windows환경에서 WSL을 사용하시는 경우, Xlaunch(VcXsrv)와 같은 가상 X 윈도우 서버 프로그램이 필요합니다.
> [VcXsrc](https://sourceforge.net/projects/vcxsrv/)

## 설치

1. 패키지 설치

```shell
composer require miniyus/laravel-tistory-api

php artisan vendor:publish --provider="Miniyus\TistoryApi\TistoryServiceProvider"
```

2. 크롬 버전 확인

```shell
#for Linux
google-chrome --version

# windows는 Chrome 브라우저에서 설정 > Chrome 정보
# or
# Chrome 브라우저 주소창에 > chrome://settings/help 
```

3. 크롬 드라이버 설치

- https://chromedriver.chromium.org/downloads
- 버전은 xx.x.xxxx.xx 형식인데 마지막 마이너 버전은 상관 없으니, 현재 버전에 맞는 드라이버가 없다고 당황하지 마십시오.
    - 가능한 경우: Driver Version: 92.0.4515.107 > Chrome Version: 92.0.4515.159
    - 불가능한 경우: Driver Version: 92.0.4515.107 > ChromeVersion: 92.0.4415.101

4. .env 파일 설정

```dotenv
TISTORY_APP_ID={티스토리에서 발급해준 APP ID}
TISTORY_API_KEY={티스토리에서 발급해준 API KEY}
TISTORY_BLOG_NAME={본인 블로그이름} # 블로그이름 = https://{블로그이름}.tistory.com
KAKAO_ACCOUNT={카카오톡로그인ID}
KAKAO_PASSWORD={카카오톡패스워드}
```

## 사용법

<details>

<summary>Login</summary>

```php
<?php
// login
$client = \Miniyus\TistoryApi\Tistory\TistoryClient::login();
```

</details>

<details>
<summary>Post</summary>

```php
<?php

// login
$client = \Miniyus\TistoryApi\Tistory\TistoryClient::login();

// https://www.tistory.com/apis/post
// 글 관련 API 모듈 가져오기
$post = $client->apis()->post();

// GET https://www.tistory.com/apis/post/list
// 작성한 포스트 리스트를 가져옵니다.
$post->list();

// POST https://www.tistory.com/apis/post/write
// 글 작성 시, \Miniyus\TistoryApi\Tistory\Data\TistoryPost 객체를 사용합니다.
$data = \Miniyus\TistoryApi\Tistory\Data\TistoryPost::newInstance();
$data->setTitle('...');
$data->setContent('...');
$response = $post->write($data);

// 읽기: GET https://www.tistory.com/apis/post/{postId}
$post->read($postId);

// 파일첨부: POST https://www.tistory.com/apis/post/attach
$post->attach($filename,$fileContent);

// 수정: POST https://www.tistory.com/apis/post/modify
// 글 작성과 마찬가지로 TistoryPost 객체를 사용합니다.
$post->modify($data);

// 요청의 성공여부 체크
if($post->isSuccess()){
    // success!
}else{
    // fail...
    // 에러 내용을 확인 할 수 있습니다.
    $post->getError();
}

// 해당 메서드는 Response객체(Illuminate\Http\Response)를 가지고 올 수 있습니다.
// 이 패키지는 내부적으로 Laravel Http 파사드를 사용합니다.
$post->getResponse();

```

</details>

###[Laravel Tistory API](https://github.com/miniyus/laravel-tistory-api)

[![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=miniyus&repo=laravel-tistory-api&show_owner=true&theme=nord)](https://github.com/miniyus/restful-api-client)
