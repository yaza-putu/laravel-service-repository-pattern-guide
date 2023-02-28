# Artisan Command
## Make Repository
Make only repository
```php
php artisan make:repository User
// or
php artisan make:repository UserRepository
```
Make Repository with service
```php
// or create together with a service
php artisan make:repository User --service
// or
php artisan make:repository UserRepository --service
```
## Make Basic Service
Make only service
```php
php artisan make:service User
// or
php artisan make:service UserService
```
Make service with repository
```php
php artisan make:service UserService --repository
```
## Make Api Service
Make service for Rest API
```php
php artisan make:service UserService --api
```