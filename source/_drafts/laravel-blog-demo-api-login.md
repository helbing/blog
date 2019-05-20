---
title: 基于 laravel 开发博客接口 - 登录接口开发
description: 在之后的这段时间里，我将写一个系列博文，基于 laravel 开发博客接口。借助这个系列博文分享我在开发 php web 项目中的一些经验，习惯和小技巧，希望能对大家有点帮助
cover: https://tuchuang-1253782374.cos.ap-guangzhou.myqcloud.com/20190208165724.png
date: 2019-02-11
tag: ["php", "laravel"]
---

把 migrate 表都删光，重新建表

```php
// 2019_02_20_000000_create_users_table.php

<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateUsersTable extends Migration
{
    private $table = 'users';

    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up(): void
    {
        Schema::create($this->table, function (Blueprint $table) {
            $table->increments('id');
            $table->string('name', 60)->default('')->comment('账号');
            $table->string('real_name', 30)->default('')->comment('真实姓名');
            $table->string('password', 60)->default('')->comment('密码');
            $table->string('avatar')->default('')->comment('头像');
            $table->timestamps();

            $table->unique('name');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down(): void
    {
        Schema::dropIfExists($this->table);
    }
}
```

```shell
composer require tymon/jwt-auth 1.0.0-rc.3
```

```shell
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

```shell
php artisan jwt:secret
```

在 .env.example 中加入

```ini
JWT_SECRET=foobar
```

保证 .env 和 .env.example 的配置是一致的

编辑 `app/Models/User.php`

```php
<?php

declare(strict_types=1);

namespace App\Models;

use Illuminate\Notifications\Notifiable;
use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Tymon\JWTAuth\Contracts\JWTSubject;

class User extends Authenticatable implements JWTSubject
{
    use Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name', 'email', 'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];

    /**
     * Get the identifier that will be stored in the subject claim of the JWT.
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * Return a key value array, containing any custom claims to be added to the JWT.
     *
     * @return array
     */
    public function getJWTCustomClaims(): array
    {
        return [];
    }
}
```

修改 `config/auth.php`

```php
'defaults' => [
    'guard' => 'api',
    'passwords' => 'users',
],

...

'guards' => [
    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
    ],
],

'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => App\Models\User::class,
        ],

        // 'users' => [
        //     'driver' => 'database',
        //     'table' => 'users',
        // ],
    ],
```
