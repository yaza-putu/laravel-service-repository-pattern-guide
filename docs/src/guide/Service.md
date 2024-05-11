# Service
## Important
> Have 2 kind service

Have 2 kind services :
- Basic Service
- Rest Api Service

> One service can call many repository, don't call model in service

Service only write bussiness logic not query logic

Service can call many repository

> Basic Operation CRUD like Create, Update, Delete, Find, FindOrFail, all already exist

You can override function in service implement class for basic operation CRUD function

## Generate
Basic Service
```php
php artisan make:service User
// or
php artisan make:service UserService
```
Create service with blank template
```php
php artisan make:service UserService --blank
```
more command [click here](./Artisan.md#make-basic-service)

## Folder Structure
>ex : Folder Structure UserService

<pre class="vue-container"><code><p>.
├── Services
│&nbsp;&nbsp; ├── User
│&nbsp;&nbsp; │&nbsp;&nbsp; ├── <code>UserService</code> <em>(<strong>Interface</strong>)</em>
│&nbsp;&nbsp; │&nbsp;&nbsp; ├── <code>UserServiceImplement</code> <em>(<strong>Class</strong>)</em>
</p>
</code></pre>


## Example Code Basic Service (Blank Template)
### Interface
```php
<?php

namespace App\Services\User;

use App\Models\User;
use LaravelEasyRepository\BaseService;

interface UserService extends BaseService{

    /**
     * find by email service
     * @param string $email
     * @return User|null
     */
    public function findByEmail(string $email) : ?User;
}
```
### Class Implement
> service implement only call repository interface not call repository class implement

Basic CRUD already exists if extend Service Class

```php
<?php

namespace App\Services\User;

use App\Models\User;
use Illuminate\Support\Facades\Log;
use LaravelEasyRepository\Service;
use App\Repositories\User\UserRepository;

class UserServiceImplement extends Service implements UserService{

     /**
     * don't change $this->mainRepository variable name
     * because used in extends service class
     */
     protected $mainRepository;

    public function __construct(UserRepository $mainRepository)
    {
      $this->mainRepository = $mainRepository;
    }

    /**
     * find by email service implement
     * @param string $email
     * @return User|null
     */
    public function findByEmail(string $email): ?User
    {
        try {
            return $this->mainRepository->findByEmail($email);
        } catch (\Exception $exception) {
            Log::error($exception);
            return null;
        }
    }
}
```

## Example Code Rest Api Service (Default Template)
### Interface
```php
<?php

namespace App\Services\User;
use LaravelEasyRepository\BaseService;

interface UserService extends BaseService{
    /**
     * response
     * @param string $email
     * @return Response
     */
    public function findByEmail(string $email) : UserService;
}
```

### Class Implement
> service implement only call repository interface not call repository class implement

Basic CRUD already exists if extend ServiceApi Class
```php
<?php

namespace App\Services\User;

use App\Repositories\User\UserRepository;
use LaravelEasyRepository\ServiceApi;
class UserServiceImplement extends ServiceApi implements UserService{

     /**
     * don't change $this->mainRepository variable name
     * because used in extends service class
     */
     protected $mainRepository;

    protected $title = "User";
    protected $create_message = "Berhasil Dibuat";
    protected $update_message = "Berhasil Diupdate";
    protected $delete_message = "Berhasil Dihapus";

    public function __construct(UserRepository $mainRepository)
    {
      $this->mainRepository = $mainRepository;
    }

    /**
     * find by email service implement
     * @param string $email
     * @return User|null
     */
    public function findByEmail(string $email) : UserService
    {
        try {
            $result = $this->mainRepository->findByEmail($email);
            return $this->setCode(200)
                        ->setMessage("OK")
                        ->setData($result);

        } catch (\Exception $exception) {
            return $this->exceptionResponse($exception);
        }
    }
}
```

## Call Basic Service In Controller
> One controller can call many service

Controller don't call repository, only call service

```php
<?php

namespace App\Http\Controllers;

use App\Services\User\UserService;
use Illuminate\Http\Request;

class UserController extends Controller
{
    protected $userService;
    public function __construct(UserService $userService)
    {
        $this->userService = $userService;
    }

    /**
     * show all
     */
    public function show()
    {
        $this->userService->findByEmail('rlegros@hotmail.com');
        // more action
    }
}
```

## Call Rest Api Service In Controller
> method ->toJson() automatic return response as json

if you need get data from response service (only extend ServiceApi class) you can call 3 method :
- ->getResult() (to get all data) (deprecated)
- ->getStatus() (to get status like true/false) (deprecated)
- ->getCode() (to get code like 200)
- ->getMessage() (to get message like success or error message)
- ->getData() (to get all data)
- ->getError() (to get errors)
- ->toJson() (to generate json response api)
```php
<?php

namespace App\Http\Controllers;

use App\Services\User\UserService;
use Illuminate\Http\JsonResponse;
use Illuminate\Http\Request;

class UserController extends Controller
{
    protected $userService;
    public function __construct(UserService $userService)
    {
        $this->userService = $userService;
    }
    
    /**
     * show all
     * @return JsonResponse
     */
    public function show() : JsonResponse
    {
        return $this->userService->findByEmail('rlegros@hotmail.com')->toJson();
    }
}

```

### Result method ->toJson()
> In version 5, message (key of json data) only for message can't use for validation error message, if you need read error message or validation error message use errors (key of json data)
// new response
```json
{
    "code": 200,
    "message": "Ok",
    "data": {
        "id": "2pockz3quxkw8s8w",
        "name": "tgrant",
        "email": "rlegros@hotmail.com",
        "email_verified_at": null,
        "details": [
            {
                "sample": "1"
            },
            {
                "sample": "2"
            },
            {
                "sample": "3"
            }
        ],
        "date": "26-02-2023",
        "created_at": "2023-02-26T06:57:34.000000Z",
        "updated_at": "2023-02-26T06:57:34.000000Z"
    },
    "errors": []
}
```
// old response (deprecated)
```json
{
    "success": true,
    "code": 200,
    "message": null,
    "data": {
        "id": "2pockz3quxkw8s8w",
        "name": "tgrant",
        "email": "rlegros@hotmail.com",
        "email_verified_at": null,
        "details": [
            {
                "sample": "1"
            },
            {
                "sample": "2"
            },
            {
                "sample": "3"
            }
        ],
        "date": "26-02-2023",
        "created_at": "2023-02-26T06:57:34.000000Z",
        "updated_at": "2023-02-26T06:57:34.000000Z"
    }
}
```