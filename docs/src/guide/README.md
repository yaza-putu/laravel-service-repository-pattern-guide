# How It Works
This package support for generate Service & Repository pattern with artisan command : 
- On service you can write bussines logic 
- On Repository you can write a query logic for database

If you use command php artisan make:service or repository, this command will generate 2 file:
- interface
- implement class

interface auto bind to class implement with container laravel, for detail you can read [read docs bind interface with implement](https://laravel.com/docs/9.x/container#binding-interfaces-to-implementations) you can call method on interface to access method in class implement.

if you need to change or modification bind interface to new implement class you can add this config to AppServiceProvider : [click here](../config/README.md#change-bind-interface-to-new-class-implement)

class implement extend with service or Elequent for handel basic method CRUD like, create, update, delete, findOrFail
you can override basic method crud for re-write code logic

> For Basic CRUD like Create, Update, Delete, Find, FindOrFail, all already exist and ready to use