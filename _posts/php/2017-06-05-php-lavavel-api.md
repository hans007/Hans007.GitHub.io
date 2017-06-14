---
layout: post
title: php - laravel 5.4  API 认证系统
category: php
tags:
  - lnmp
  - laravel
---

## 安装

```
composer require laravel/passport
```

## 配置

```
vi config/app.php

-> providers

Laravel\Passport\PassportServiceProvider::class,

```

## 数据库安装

```
php artisan migrate
```

- `1071坑`

错误信息

```
  SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes (SQL: alter table   
  `users` add unique `users_email_unique`(`email`))     
```

修正, 需要设置默认字符串字段长度

```
vi aoo/Providers/AppServiceProvider.php

use Illuminate\Support\Facades\Schema;

...

public function boot()
{
    //
    Schema::defaultStringLength(191);
}

```

成功信息

```
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table
Migrating: 2016_06_01_000001_create_oauth_auth_codes_table
Migrated:  2016_06_01_000001_create_oauth_auth_codes_table
Migrating: 2016_06_01_000002_create_oauth_access_tokens_table
Migrated:  2016_06_01_000002_create_oauth_access_tokens_table
Migrating: 2016_06_01_000003_create_oauth_refresh_tokens_table
Migrated:  2016_06_01_000003_create_oauth_refresh_tokens_table
Migrating: 2016_06_01_000004_create_oauth_clients_table
Migrated:  2016_06_01_000004_create_oauth_clients_table
Migrating: 2016_06_01_000005_create_oauth_personal_access_clients_table
Migrated:  2016_06_01_000005_create_oauth_personal_access_clients_table
```

## 配置key

```
php artisan passport:install

Personal access client created successfully.
Client ID: 1
Client Secret: 0rUr7KwBvfCoAvU8PAnhYjkS3N6HdPf8OlZBMMT8
Password grant client created successfully.
Client ID: 2
Client Secret: YdY3q364Sm2K6UqV2HhkJqaE3yW9oeZ1ksg3VMjV
```



## 

