# CRUD_SKELETON_LARAVEL_5.2

[![Build Status](https://travis-ci.org/laravel/framework.svg)](https://travis-ci.org/laravel/framework)
[![Total Downloads](https://poser.pugx.org/laravel/framework/d/total.svg)](https://packagist.org/packages/laravel/framework)
[![Latest Stable Version](https://poser.pugx.org/laravel/framework/v/stable.svg)](https://packagist.org/packages/laravel/framework)
[![Latest Unstable Version](https://poser.pugx.org/laravel/framework/v/unstable.svg)](https://packagist.org/packages/laravel/framework)
[![License](https://poser.pugx.org/laravel/framework/license.svg)](https://packagist.org/packages/laravel/framework)

# Why should you use CRUD_SKELETON
- It will help you easy to```Create,Read,Update,Delete``` data in any table type, include your table has ```file field```, ```foriegn key```
# How to run skeleton?

- clone this source code, in ```.env``` file update your database info.For example
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=CRUD_example
DB_USERNAME=root
DB_PASSWORD=
```
- open terminal and migrate database 
```php artisan migrate```
- open browser and past this link 
```http://localhost/YOUR__SERVER_FOLDER/public/admin/category```
- if your browser cant load page, please make sure you changed mod storage and bootstrap to 777
```
chmod -R 777 storage/
chmod -R 777 bootstrap/
```
your browser should display like this 

![untitled](https://cloud.githubusercontent.com/assets/26756140/24553748/6f1e3da4-1655-11e7-9ee4-a99e1e465dea.png)

# How does it work?
### Here's our database schema
![untitled](https://cloud.githubusercontent.com/assets/26756140/24806532/5cedac98-1bdf-11e7-9ff0-154883e4261b.png)
### To create CRUD table 

#### 1. you need provide some info : Get and Post router, uniqueField in your table, private key of your table,validate form when create new data and validate form when update data
Please open ```CategoryController``` to get example : 

```php
    private $mRouter = ['GET' => 'get_category', 'POST' => 'post_category'];
    private $uniqueFields = array('name');
    private $privateKey = 'id';
    private $validateForm = ['name'=>'required|max:255'];
    private $validateFormUpdate = ['name'=>'required|max:255'];
```
if you have file field you need provide file field and file path in your table 
For example : 
```php
private $fieldFile = array('image');
private $fieldPath = array('image_path');
```
*note : you must use ```_path``` prefix in your field path

if you have foreign key in your table you must provide foreignData array follow format and return it value to view use ```foreignData``` key
Please open ```NewPostController``` to get example
```php
$category = ['fr_id' =>'category_id',
            'fr_data'=>$this->getDataByModel(new Category()),
            'fr_private_id' =>'id',
            'fr_select_field' => 'name',
            'label' => 'Category'
        ];
$this->foreignData = [$category];
```
#### 2. Progress POST (CREATE and UPDATE data) :
  - Handle all data you need create from your form 
  ```php
  $active = !empty($request->get('active')) ? 1 : 0 ;
  $progressData = ['active' => $active,'name' => $request->get('name')];
  ```
  if you have file field in table you need progreesFileData
  ```php 
  $progressData =  $this->progressFileData($request,$this->fieldFile,$progressData);
  ```          
  - Process Post data and get validate return to display at view
  
  ```php
  $this->validateMaker = $this->progressPost($request,$progressData)->parseMessageToValidateMaker();
  ```
#### 3. Process Get (READ and DELETE data)
 - Just provide request data to progressGet function and get return validate info 
```php 
$this->validateMaker = $this->progressGet($request)->parseMessageToValidateMaker();
```
### finally you will have all CRUD function like this
![untitled](https://cloud.githubusercontent.com/assets/8258900/24807525/b6dc85c8-1be2-11e7-8c0d-57dbcf40dbd4.png)

##### Please do not hesitate to let me know if you have any questions thongds@gmail.com
