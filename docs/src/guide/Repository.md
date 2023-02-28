# Repository
## Important
> Single Responsibility

Don't Call many model in one repository

> Basic CRUD like Create, Update, Delete, Find, FindOrFail, All <br>

Already exist don't create again, you can override basic CRUD function in class implement
> Don't write business logic

Repository only write query logic not business logic

## Generate
```php
php artisan make:repository User
// or
php artisan make:repository UserRepository
```
more command [click here](./Artisan.md#make-repository)

## Folder Structure
>ex : Folder Structure UserRepository

<pre class="vue-container"><code><p>.
├── Repositories
│&nbsp;&nbsp; ├── User
│&nbsp;&nbsp; │&nbsp;&nbsp; ├── <code>UserRepository</code> <em>(<strong>Interface</strong>)</em>
│&nbsp;&nbsp; │&nbsp;&nbsp; ├── <code>UserRepositoryImplement</code> <em>(<strong>Class</strong>)</em>
</p>
</code></pre>

## Example Code
### Interface
```php
<?php

namespace App\Repositories\User;

use App\Models\User;
use LaravelEasyRepository\Repository;

interface UserRepository extends Repository{

    /**
     * find data by email
     * @param string $email
     * @return User
     */
    public function findByEmail(string $email) : User;
}

```
### Class Implement
You can override basic CRUD function in class implement
```php
<?php

namespace App\Repositories\User;

use LaravelEasyRepository\Implementations\Eloquent;
use App\Models\User;

class UserRepositoryImplement extends Eloquent implements UserRepository{

    /**
    * Model class to be used in this repository for the common methods inside Eloquent
    * Don't remove or change $this->model variable name
    * @property Model|mixed $model;
    */
    protected $model;

    public function __construct(User $model)
    {
        $this->model = $model;
    }

     /**
     * find data by email
     * @param string $email
     * @return User
     */
    public function findByEmail(string $email): User
    {
        return $this->model->where('email', $email)->first();
    }
}

```

## How To Call In Service
Important
> Interface auto bind to class implement 
 
You only call interface not class implement

now you call Repository(interface) in Class Service Implement
```php
<?php

namespace App\Services\User;

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
        } catch (Exception $exception) {
            Log::error($exception);
            return null;
        }
    }
}

```