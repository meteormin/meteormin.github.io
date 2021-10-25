---
title: Restful Api Client
excerpt: My PHP Repositories
categories: php
---

## RestfulApiClient For Laravel

laravel용 restful API개발 도구입니다. restful API의 url구조를 클래스로 구현하기 쉽게 Abstract 클래스들을 제공합니다.

## Preview

```php
<?php
// GET https://api.exmaple.com/v1/user
$response = ApiClient::v1()->user()->get();

// POST https://api.example.com/v1/user
$request = ['something'=>''];
$response = ApiCLient::v1()->user()->post($request);

// PUT https://api.example.com/v1/user
$request = ['something'=>''];
$response = ApiCLient::v1()->user()->put($request);

// if you have path parameter
// PUT https://api.example.com/v1/user/1 
$id = 1;
$request = ['something'=>''];
$response = ApiCLient::v1()->user()->put($id, $request);

// DELETE https://api.example.com/v1/user
$response = ApiCLient::v1()->user()->delete();

// if you have path parameter
// DELETE https://api.example.com/v1/user/1
$id = 1;
$response = ApiCLient::v1()->user()->delete($id);
```