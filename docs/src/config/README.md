---
sidebar: auto
---

# Config
## Publish Config
```php
php artisan vendor:publish --provider="LaravelEasyRepository\LaravelEasyRepositoryServiceProvider" --tag="easy-repository-config"
```
## Config Location 
Config.php
> config > easy-repository.php

## Change Root Directory Of Service
> If Change Directory you need change namespace
```php
<?php

return [
.....
   /**
     * The directory for all the services
     */
    "service_directory" => "app/Services",
    
     /**
     * Default service namespace
     */
    "service_namespace" => "App\Services",
.....
```

## Change Root Directory Of Repository
> If Change Directory you need change namespace

```php
<?php

return [
......
  /**
     * The directory for all the repositories
     */
    "repository_directory" => "app/Repositories",

    /**
     * Default repository namespace
     */
    "repository_namespace" => "App\Repositories",
......
```

## Change Bind Interface to New Class Implement
Add this config to AppServiceProvider
```php
$this->app->extend(Interface::class, function ($service, $app) {
        return new NewImplement($service);
    });
```