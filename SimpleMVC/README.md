# Topic 03: Creating simple MVC application in PHP
Under this topic, we tackle series of lessons that eventually build up into a simple model-view-controller applciation in PHP on MySQL database server.

We will begin the topic by defining model objects that represent our database entities. We will add a simple configuration to establish a connection to our database.
With that in place, we then look at adding repository classes to help manage the resources in our data storage.

And then, we will create controller classes to convey data resources to the web views.

## Table of Contents
+ [Lesson 01](#01-application-directory-structure): Application directory structure
+ [Lesson 02](#02-defining-application-configurations): Defining application configurations
+ [Lesson 03](#02-defining-model-objects): Defining model objects
+ [Lesson 04](#03-database-connection-configuration): Database connection configuration
+ Lesson 05: Data resource repositories
+ Lesson 06: Controllers
+ Lesson 07: View templates
+ Lesson 08: Route configuration
+ Lesson 09: User session configuration
+ Lesson 10: Conclusion


## 01: Application Directory Structure

> Introduction

To have a very simple and clean project outlook, we need to group our files into various functionalities.
First and foremost, we shall create a parent (**root** or **working**) directory named `SimpleMVC`. This folder will serve as the root or starting directory for the whole project.
Inside the root directory, we shall have the following subdirectories: `configs`, `controllers`, `models`, `repositories` and `views`.
In addition to the directories, we shall have three other important files - `index.php`, `routes.php` and `.htaccess`.

We will come to understand the purpose each of these directories and files is serving as we move along.

> SimpleMVC

* [src/](./src)
  * [configs/](./src/configs)
    * [ConstantsConfig.php](./src/configs/ConstantsConfig.php)
    * [DatabaseConfig.php](./src/configs/DatabaseConfig.php)
    * [RouteConfig.php](./src/configs/RouteConfig.php)
    * [SessionConfig.php](./src/configs/SessionConfig.php)
  * [controllers/](./src/controllers)
    * [UsersController.php](./src/controllers/UsersController.php)
  * [models/](./src/models)
    * [UserModel.php](./src/models/UserModel.php)
  * [repositories/](./src/repositories)
    * [UsersRepository.php](./src/repositories/UsersRepository.php)
  * [views/](./src/views)
    * [assets/](./src/views/assets)
      * [css/](./src/views/assets/css)
        * [style.css](./src/views/assets/css/style.css)
      * [images/](./src/views/assets/images)
        * [logo.png](./src/views/assets/images/logo.png)
      * [js/](./src/views/assets/js)
        * [main.js](./src/views/assets/js/main.js)
    * [modules/](./src/views/modules)
      * [about.js](./src/views/modules/about.js)
      * [home.js](./src/views/modules/home.js)
    * [about.php](./src/views/about.php)
    * [home.php](./src/views/home.php)
  * [.htaccess](./src/.htaccess)
  * [index.php](./src/index.php)
  * [routes.php](./src/routes.php)

> NOTE: You notice that the `controllers`, `models` and `repositories` directories have just **one** file in each of them. They are so because this project is for demonstration purpose. A real application will have quite a number of files in these folders.

<br>

## 02: Defining Application Configurations

<br>
To get things simple and clean, let centralize the constants and other configurations that will the various components of our application will be consuming.

<br>

> ConstantsConfig.php

Let's begin by defining our constants. We define constants for `database_host`, `database_name`, `database_user` and `database_pass`.

Again, we will also define another constants `user_session` for storing user session data.
> DatabaseConfig.php
```
<?php 
class ConstantsConfig {

    // Database Credentials
    const $database_host = "localhost";
    const $database_user = "root";
    const $database_pass = "";
    const $database_name = "address_book";

    // Session Names
    const $user_session_id = "user_session";

    ...
    ...
}
```

That's it for `ConstantsConfig.php` for now. When you need other constants along the way, we will come back to this file to define them.


## 03: Defining Model Objects

> Introduction

By `model`, I'm referring to the `class` representation of the database objects (tables, views, etc.). In essence, I mean to say that, every table in our database will be represented as a PHP class with the corresponding fields as static fields in the class.

> UserModel

```
  <?php
  class UserModel {

      // Data object fields
      private static $id;
      private static $username;
      private static $email_address;
      private static $phone_number;
      private static $is_admin;
      private static $is_active;
      private static $created_at;
      private static $updated_at;

      // Object setter
      public static function set($id, $username, $email_address, $phone_number, $is_admin, $is_active, $created_at, $updated_at){
          self::$id = $id;
          self::$username = $username;
          self::$email_address = $email_address;
          self::$phone_number = $phone_number;
          self::$password = $password;
          self::$is_admin = $is_admin;
          self::$is_active = $is_active;
          self::$created_at = $created_at;
          self::$updated_at = $updated_at;
      }

      // Obejct getter
      public static function get(){
          return array(
              "id" => self::$id,
              "username" => self::$username,
              "email_address" => self::$email_address,
              "phone_number" => self::$phone_number,
              "is_admin" => self::$is_admin,
              "is_active" => self::$is_active,
              "created_at" => self::$created_at,
              "updated_at" => self::$updated_at,
          );
      }

  }
```

> NOTE: I ommited the `password` field from the model class. This is because, the `password` will not be send to the view template.

## 04: Database Connection Configuration