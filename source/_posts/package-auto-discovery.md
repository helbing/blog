---
title: 深入了解 Laravel 5.5 包自动发现
description: laravel5.5 开始提供了一个新特性，包自动发现 Package Auto Discuvery，包自动发现主要用于自动发现第三方包的 ServiceProvider 和 Facade 并进行注册
cover: https://tuchuang-1253782374.cos.ap-guangzhou.myqcloud.com/20190211132629.png
date: 2019-11-03
tag: ["php", "laravel"]
---

## 使用

第三方包只需要在自己的 composer.json 文件中加入需要注册的 ServiceProvider 和 Facade，laravel 的 package auto discovery 就会自动发现并注册

```json
"extra": {
    "laravel": {
        "providers": [
            "Barryvdh\\Debugbar\\ServiceProvider"
        ],
        "aliases": {
            "Debugbar": "Barryvdh\\Debugbar\\Facade"
        }
    }
},
```

如果你不想 laravel 对某个包做自动发现，你可以在你 laravel 项目的 composer.json 中加入

```json
"extra": {
    "laravel": {
        "dont-discover": "barryvdh/laravel-debugbar"
    }
},
```

## 原理

```json
"scripts": {
    "post-root-package-install": [
        "@php -r \"file_exists('.env') || copy('.env.example', '.env');\""
    ],
    "post-create-project-cmd": [
        "@php artisan key:generate --ansi"
    ],
    "post-autoload-dump": [
        "Illuminate\\Foundation\\ComposerScripts::postAutoloadDump",
        "@php artisan package:discover --ansi"
    ]
},
```

laravel 的 package auto discovery 主要是通过`php artisan package:discover --ansi`这条命令来执行的，这条命令位于`Illuminate\Foundation\Console\PackageDiscoverCommand`这个类中，这个类会调用`Illuminate\Foundation\PackageManifest`的 build 方法来做自动发现

```php
public function build()
{
    $packages = [];

    // installed.json是composer在安装完所有包后会自动生成的文件，其实就是所有包的composer.json的集合
    if ($this->files->exists($path = $this->vendorPath . '/composer/installed.json')) {
        $packages = json_decode($this->files->get($path), true);
    }

    // $this->packagesToIgnore()会获取到当前laravel项目的composer.json中不需要做自动发现的包
    $ignoreAll = in_array('*', $ignore = $this->packagesToIgnore());

    // 这个很长的调用其实是获取到每一个需要自动注册的ServiceProvider和Facade
    // 然后将结果作为参数传入write方法
    $this->write(collect($packages)->mapWithKeys(function ($package) {
        return [$this->format($package['name']) => $package['extra']['laravel'] ?? []];
    })->each(function ($configuration) use (&$ignore) {
        $ignore = array_merge($ignore, $configuration['dont-discover'] ?? []);
    })->reject(function ($configuration, $package) use ($ignore, $ignoreAll) {
        return $ignoreAll || in_array($package, $ignore);
    })->filter()->all());
}

protected function write(array $manifest)
{
    // $this->manifestPath这个的值从哪里来的？你可以去看Illuminate\Foundation\Application的registerBaseBindings方法
    // $this->manifestPath的值一般会是bootstrap/cache/packages.php
    if (!is_writable(dirname($this->manifestPath))) {
        throw new Exception('The ' . dirname($this->manifestPath) . ' directory must be present and writable.');
    }

    // 单纯的把自动发现到的ServiceProvider和Facade写入到文件中
    $this->files->replace(
        $this->manifestPath,
        '<?php return ' . var_export($manifest, true) . ';'
    );
}
```

然后这条命令就算是执行完成了，单纯的只是做了自动发现，那么自动注册呢？

全局搜索 PackageManifest，可以发现`Illuminate\Foundation\Application`中的 registerConfiguredProviders 有调用到

```php
public function registerConfiguredProviders()
{
    // 获取到app.php中所有需要注册的ServiceProvider
    $providers = Collection::make($this->config['app.providers'])
        ->partition(function ($provider) {
            return Str::startsWith($provider, 'Illuminate\\');
        });

    // 获取到自动发现到的所有需要注册的ServiceProvider
    $providers->splice(1, 0, [$this->make(PackageManifest::class)->providers()]);

    // 这里函数调用会去注册ServiceProvider
    (new ProviderRepository($this, new Filesystem(), $this->getCachedServicesPath()))
        ->load($providers->collapse()->toArray());
}
```

继续全局搜索 PackageManifest，可以发现`Illuminate\Foundation\Bootstrap\RegisterFacades`中的 bootstrap 有调用到

```php
public function bootstrap(Application $app)
{
    Facade::clearResolvedInstances();

    Facade::setFacadeApplication($app);

    // 注册Facade
    AliasLoader::getInstance(array_merge(
        // 获取到app.php中所有需要注册的Facade
        $app->make('config')->get('app.aliases', []),
        // 获取到自动发现到的所有需要注册的Facade
        $app->make(PackageManifest::class)->aliases()
    ))->register();
}
```
