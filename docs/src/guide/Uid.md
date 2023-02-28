# Make Primary Key As UID string
> This package include auto generate uid as primary key

## Setup in migration
From
```php
public function up(): void
{
 Schema::create('users', function (Blueprint $table) {
            $table->id();
            .....
 });
}
```
TO
```php
public function up(): void
{
 Schema::create('users', function (Blueprint $table) {
            $table->uuid('id')->primary();
            .....
 });
}
```

Set relation if foreign key as uid in migration
```php
public function up(): void
{
 Schema::create('transactions', function (Blueprint $table) {
    ........ 
    $table->foreignUuid('user_id')->references('users')->onDelete('cascade')->onUpdate('cascade');
    .....
 });
}
```

## Setup In Model
> use Trait GenUid

```php
<?php

namespace App\Models;

use LaravelEasyRepository\Traits\GenUid;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable, GenUid;
    ...........
}
```

## Result
> Auto Generate Uid in ID in store data to database (Mutator)

| id               | name   | email               | email_verified_at | password                                                     | details                                             | date       | remember_token | created_at          | updated_at          |
|------------------|--------|---------------------|-------------------|--------------------------------------------------------------|-----------------------------------------------------|------------|----------------|---------------------|---------------------|
| 2pockz3quxkw8s8w | tgrant | rlegros@hotmail.com |                   | $2y$10$K9RAQjamEU4xNh7PrLs4zOIARiwZ4h14VpqnnklbjzGDZKrwYcKdC | [{"sample": "1"}, {"sample": "2"}, {"sample": "3"}] | 2023-02-26 |                | 2023-02-26 06:57:34 | 2023-02-26 06:57:34 |
