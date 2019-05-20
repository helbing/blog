---
title: 基于 laravel 开发博客接口 - 准备工作
description: 在之后的这段时间里，我将写一个系列博文，基于 laravel 开发博客接口。借助这个系列博文分享我在开发 php web 项目中的一些经验，习惯和小技巧，希望能对大家有点帮助
cover: https://tuchuang-1253782374.cos.ap-guangzhou.myqcloud.com/20190208165724.png
date: 2019-02-11
tag: ["php", "laravel"]
---

由于我本人使用的 MacBook Pro，所以很多操作仅适用于 macOS，但是其实在 Windows 上也是大同小异，所以如果你使用的是 Windows 电脑就需要你做些调整了

## 项目说明

## 开发环境搭建

### php 安装

```shell
brew install php
```

### composer, php-cs-fixer, phpcs, phpmd, phpunit 安装

```shell
brew install composer
brew install php-cs-fixer
brew install php-code-sniffer
brew install phpmd
brew install phpunit
```

### xdebug 安装

```shell
pecl install xdebug
```

配置 xdebug.ini

```ini
[xdebug]
zend_extension=xdebug.so
xdebug.default_enable=1
xdebug.remote_enable=1
xdebug.remote_autostart=1
xdebug.remote_host=127.0.0.1
xdebug.remote_port=9001
xdebug.remote_handler="dbgp"
xdebug.idekey=VSCODE
xdebug.show_exception_trace=0
xdebug.var_display_max_depth=8
```

### docker & laradock 安装

```shell
brew cask install docker
```

## 配置 vscode

### 安装 vscode

```shell
brew cask install visual-studio-code
```

### 插件安装

1. [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)
2. [php cs fixer](https://marketplace.visualstudio.com/items?itemName=junstyle.php-cs-fixer)
3. [PHP Debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug)
4. [PHP DocBlocker](https://marketplace.visualstudio.com/items?itemName=neilbrayfield.php-docblocker)
5. [PHP Mess Detector](https://marketplace.visualstudio.com/items?itemName=ecodes.vscode-phpmd)
6. [phpcs](https://marketplace.visualstudio.com/items?itemName=ikappas.phpcs)
7. [PHPUnit](https://marketplace.visualstudio.com/items?itemName=emallin.phpunit)
8. [EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)

### 配置

```json
// php
"php.suggest.basic": false,
"php.validate.executablePath": "/usr/local/bin/php",
"php-cs-fixer.onsave": true,
"php-cs-fixer.rules": "@PSR2",
"php-cs-fixer.lastDownload": 1546395672996,
"phpcs.enable": true,
"phpcs.standard": "PSR2",
"phpcs.showSources": true,
"phpcs.trace.server": "verbose",
"phpcs.ignorePatterns": ["*/vendor/*"],
"phpcs.autoConfigSearch": true,
"phpmd.enabled": true,
```

## 项目脚手架

在开始搭建项目前，我们需要把脚手架先搭建好，方便后续开发项目

### 使用 composer 创建项目

```shell
composer create-project laravel/laravel blog-demo-api
cd blog-demo-api
composer install
cp .env.example .env
php artisan key:generate
```

### 安装 laravel-ide-helper

这个扩展包可以辅助 vscode 自动补全 laravel 的代码

```shell
composer require --dev barryvdh/laravel-ide-helper
composer require --dev doctrine/dbal
```

```json
// composer.json

"scripts":{
    "post-update-cmd": [
        "Illuminate\\Foundation\\ComposerScripts::postUpdate",
        "php artisan ide-helper:generate",
        "php artisan ide-helper:meta"
    ]
},
```

```shell
php artisan vendor:publish --provider="Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider"
```

```php
'write_eloquent_model_mixins' => true,
'include_helpers' => true
'model_camel_case_properties' => true,

```

### cors

```shell
composer require barryvdh/laravel-cors
php artisan vendor:publish --provider="Barryvdh\Cors\ServiceProvider"
```

```php
// app/Http/Kernel.php

protected $middleware = [
    \Barryvdh\Cors\HandleCors::class,
];
```

```php
// config/cors.php

'supportsCredentials' => true,
'allowedOrigins' => [],
'allowedOriginsPatterns' => [
    '/localhost(:\d+)?/',
],
```

根据自己的需要可以配置其他网站可以访问，因为我们是本地开发就只能保证 localhost 能正常请求

## phpcs

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ruleset name="Rules">
    <file>app</file>
    <file>config</file>
    <file>routes</file>
    <file>tests</file>
    <exclude-pattern>*/_ide_helper.php</exclude-pattern>
    <exclude-pattern>*/.phpstorm.meta.php</exclude-pattern>
    <exclude-pattern>database/*</exclude-pattern>
    <arg name="report" value="summary" />
    <arg name="colors" />
    <arg value="p" />
    <ini name="memory_limit" value="2048M" />
    <rule ref="PSR2" />
</ruleset>
```

## phpmd

```xml
<?xml version="1.0"?>
<ruleset name="Codepku Standards" xmlns="http://pmd.sf.net/ruleset/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://pmd.sf.net/ruleset/1.0.0
                     http://pmd.sf.net/ruleset_xml_schema.xsd" xsi:noNamespaceSchemaLocation="
                     http://pmd.sf.net/ruleset_xml_schema.xsd">
    <description>
        PHPMD rule sets to check php codes.
    </description>
    <rule ref="rulesets/cleancode.xml">
        <exclude name="BooleanArgumentFlag" />
        <exclude name="ElseExpression" />
        <exclude name="StaticAccess" />
    </rule>
    <rule ref="rulesets/codesize.xml" />
    <rule ref="rulesets/controversial.xml" />
    <rule ref="rulesets/design.xml" />
    <rule ref="rulesets/naming.xml" />
    <rule ref="rulesets/unusedcode.xml" />
</ruleset>
```

## git commit message

```shell
yarn global add commitizen cz-conventional-changelog
```

```shell
yarn add --dev husky@next standard-version
```

```json
// package.json

"husky": {
    "hooks": {
      "commit-msg": "commitlint -e $GIT_PARAMS"
    }
},
"scirpt": {
    "release": "standard-version"
}
```

## 其他

```txt
// .gitignore

_ide_helper.php
.phpstorm.meta.php
.DS_Store
```

```txt
// .gitattributes

/composer.lock -merge
```

上面这个配置可以避免 git 对 composer.lock 自动 merge ，这在协同开发的时候非常有用

新建 app/helpers.php 作为公共函数，然后在 composer.json 中加入

```json
"autoload": {
    "files": [
        "app/helpers.php"
    ]
},
```

```shell
composer dumpautoload
```

## postman

```shell
brew cask install postman
```
