<p style="width: 30em;text-align:left;">

![image](https://github.com/Mekaeil/LaravelUserManagement/blob/master/src/Public/mekaeils-package/images/logo-user-management.jpg)

</p>

<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/license.svg" alt="License"></a>

## About Laravel User Management

Manage all of the users, we use ["spatie/laravel-permission"](https://github.com/spatie/laravel-permission) package for manage all of the users. 
When you installing this package the "spatie/laravel-permission" package and dependencies will be install automatically.
In "LaravelUserManagement" package we create all of the DB Tables, Entities, Seeders, View for manage users, roles, permissions and departments.

# Installation

1. Install the package via composer:
```
composer require mekaeil/laravel-user-management
```
2. Add the service providers in your config/app.php file:
```
'providers' => [
    // ...
    \Mekaeil\LaravelUserManagement\LaravelUserManagementProvider::class,
];
```
3. Run this command for publish vendor:
```
php artisan vendor:publish --provider="Mekaeil\LaravelUserManagement\LaravelUserManagementProvider" 
```
4. After publishing vendors, add this code to "run" method in <b>database/DatabaseSeeder.php</b>
```
public function run()
{
    /*
    |--------------------------------------------------------------------------
    |  SEEDERS FOR LARAVEL USER MANAGEMENT
    |--------------------------------------------------------------------------
    |
    */
        $this->call(RoleTableSeeder::class);
        $this->call(PermissionTableSeeder::class);
        $this->call(DepartmentTableSeeder::class);
}
```

5. Now it's important to change config if you want(laravel_user_management): (you can skip it)
```
/*  
|--------------------------------------------------------------------------
| LARAVEL USER MANAGEMENT CONFIG
|--------------------------------------------------------------------------
|   
|
*/
    // laravel_user_management.users_table
    'users_table'           => 'users',
    // laravel_user_management.user_department_table
    'user_department_table' =>  'user_departments',

    /** 
        * THIS TABLE IS NAME OF THE MANY TO MANY RELATIONAL TABLE 
        * BETWEEN USERS TABLE & USER DEPARTMENTS TABLE
        * **/
    // laravel_user_management.user_department_user_table
    'user_department_user_table' =>  'user_departments_users',

    // laravel_user_management.password_resets_table
    'password_resets_table'      => 'user_password_resets',
    
    // laravel_user_management.user_model    
    'user_model'            => App\Entities\User::class,

    // laravel_user_management.row_list_per_page
    'row_list_per_page'     => 15,

    // laravel_user_management.admin_url
    'admin_url'             => env('APP_URL').'/admin',

    // laravel_user_management.logo_url
    'logo_url'=> env('APP_URL'). "/mekaeils-package/images/logo-user-management.jpg",
    
    'auth'  => [

        // laravel_user_management.auth.enable    
        'enable'        => true,

        // laravel_user_management.auth.login_url    
        'login_url'     => 'user/login',

        // laravel_user_management.auth.register_url    
        'register_url'  => 'user/register',

        // laravel_user_management.auth.logout_url    
        'logout_url'    => 'user/logout',
        
        // laravel_user_management.auth.username  
        'username'      => 'email', // email OR mobile 
        
        /** 
            *  DEFAULT ROLE FOR USERS WANT TO REGISTER ON WEBSITE
            *  YOU SHOULD DEFINE THIS ROLE IN SEEDER OR CREATE IT IN ADMIN PANEL
            * **/
        // laravel_user_management.auth.user_default_role  
        'user_default_role' => 'User',

        /** 
            *  DEFAULT STATUS FOR USERS WANT TO REGISTER ON WEBSITE
            *  IF IT'S SET ON 'PENDING' USER CAN NOT LOGIN IN WEBSITE 
            *  AND NEED TO ACCEPT BY ADMINISTRATOR
            * **/
        //  laravel_user_management.auth.default_user_status
        'default_user_status'   =>'accepted', /// 'pending','accepted','blocked' 
        
        // laravel_user_management.auth.dashboard_route_name_user_redirection
        'dashboard_route_name_user_redirection'  => 'home'      /// ** ROUTE NAME **       
    ],

```
6. And if set permissions table if you want to customize it: (you can skip it)
```
'models' => [

    /*
    * When using the "HasPermissions" trait from this package, we need to know which
    * Eloquent model should be used to retrieve your permissions. Of course, it
    * is often just the "Permission" model but you may use whatever you like.
    *
    * The model you want to use as a Permission model needs to implement the
    * `Spatie\Permission\Contracts\Permission` contract.
    */

    // 'permission' => Spatie\Permission\Models\Permission::class,
    'permission' => Spatie\Permission\Models\Permission::class,

    /*
    * When using the "HasRoles" trait from this package, we need to know which
    * Eloquent model should be used to retrieve your roles. Of course, it
    * is often just the "Role" model but you may use whatever you like.
    *
    * The model you want to use as a Role model needs to implement the
    * `Spatie\Permission\Contracts\Role` contract.
    */

    // 'role' => Spatie\Permission\Models\Role::class,
    'role' => Spatie\Permission\Models\Role::class,

],

'table_names' => [

    /*
    * When using the "HasRoles" trait from this package, we need to know which
    * table should be used to retrieve your roles. We have chosen a basic
    * default value but you may easily change it to any table you like.
    */

    'roles' => 'roles',

    /*
    * When using the "HasPermissions" trait from this package, we need to know which
    * table should be used to retrieve your permissions. We have chosen a basic
    * default value but you may easily change it to any table you like.
    */

    'permissions' => 'permissions',

    /*
    * When using the "HasPermissions" trait from this package, we need to know which
    * table should be used to retrieve your models permissions. We have chosen a
    * basic default value but you may easily change it to any table you like.
    */

    'model_has_permissions' => 'model_has_permissions',

    /*
    * When using the "HasRoles" trait from this package, we need to know which
    * table should be used to retrieve your models roles. We have chosen a
    * basic default value but you may easily change it to any table you like.
    */

    'model_has_roles' => 'model_has_roles',

    /*
    * When using the "HasRoles" trait from this package, we need to know which
    * table should be used to retrieve your roles permissions. We have chosen a
    * basic default value but you may easily change it to any table you like.
    */

    'role_has_permissions' => 'role_has_permissions',
],

'column_names' => [

    /*
    * Change this if you want to name the related model primary key other than
    * `model_id`.
    *
    * For example, this would be nice if your primary keys are all UUIDs. In
    * that case, name this `model_uuid`.
    */

    'model_morph_key' => 'model_id',
],

/*
* When set to true, the required permission/role names are added to the exception
* message. This could be considered an information leak in some contexts, so
* the default setting is false here for optimum safety.
*/

'display_permission_in_exception' => false,

'cache' => [

    /*
    * By default all permissions are cached for 24 hours to speed up performance.
    * When permissions or roles are updated the cache is flushed automatically.
    */

    'expiration_time' => \DateInterval::createFromDateString('24 hours'),

    /*
    * The cache key used to store all permissions.
    */

    'key' => 'spatie.permission.cache',

    /*
    * When checking for a permission against a model by passing a Permission
    * instance to the check, this key determines what attribute on the
    * Permissions model is used to cache against.
    *
    * Ideally, this should match your preferred way of checking permissions, eg:
    * `$user->can('view-posts')` would be 'name'.
    */

    'model_key' => 'name',

    /*
    * You may optionally indicate a specific cache driver to use for permission and
    * role caching using any of the `store` drivers listed in the cache.php config
    * file. Using 'default' here means to use the `default` set in cache.php.
    */

    'store' => 'default',
],
```

7. update your config/auth.php file:
```
use App\Entities\User;

return [

    /*
    |--------------------------------------------------------------------------
    | Authentication Defaults
    |--------------------------------------------------------------------------
    |
    | This option controls the default authentication "guard" and password
    | reset options for your application. You may change these defaults
    | as required, but they're a perfect start for most applications.
    |
    */

    'defaults' => [
        'guard'     => 'web',
        'passwords' => 'users',
    ],

    /*
    |--------------------------------------------------------------------------
    | Authentication Guards
    |--------------------------------------------------------------------------
    |
    | Next, you may define every authentication guard for your application.
    | Of course, a great default configuration has been defined for you
    | here which uses session storage and the Eloquent user provider.
    |
    | All authentication drivers have a user provider. This defines how the
    | users are actually retrieved out of your database or other storage
    | mechanisms used by this application to persist your user's data.
    |
    | Supported: "session", "token"
    |
    */

    'guards' => [
        'web' => [
            'driver'    => 'session',
            'provider'  => 'users',
        ],

        'api' => [
            'driver'    => 'token',
            'provider'  => 'users',
            'hash'      => false,
        ],
    ],

    /*
    |--------------------------------------------------------------------------
    | User Providers
    |--------------------------------------------------------------------------
    |
    | All authentication drivers have a user provider. This defines how the
    | users are actually retrieved out of your database or other storage
    | mechanisms used by this application to persist your user's data.
    |
    | If you have multiple user tables or models you may configure multiple
    | sources which represent each model / table. These sources may then
    | be assigned to any extra authentication guards you have defined.
    |
    | Supported: "database", "eloquent"
    |
    */

    'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model'  => User::class,
        ],

        // 'users' => [
        //     'driver' => 'database',
        //     'table' => 'users',
        // ],
    ],

    /*
    |--------------------------------------------------------------------------
    | Resetting Passwords
    |--------------------------------------------------------------------------
    |
    | You may specify multiple password reset configurations if you have more
    | than one user table or model in the application and you want to have
    | separate password reset settings based on the specific user types.
    |
    | The expire time is the number of minutes that the reset token should be
    | considered valid. This security feature keeps tokens short-lived so
    | they have less time to be guessed. You may change this as needed.
    |
    */

    'passwords' => [
        'users' => [
            'provider'  => 'users',
            'table'     => config('laravel_user_management.password_resets_table'),
            'expire'    => 60,
        ],
    ],

];
```

8. After all of the steps run these commands ordinary.
```
    5.1     php artisan migrate
    5.2     php artisan db:seed
```

9. If you want to use Vue.js, change "laravel_user_management" config file:

```
        /** 
         *  IN THIS PACKAGE WE USE THE VUE.JS FOR PAGES IF YOU 
         *  WANT TO USE IT, ENABLE IT AND FOLLOW INSTALLATION STEPS IN README FILE.
         * **/       

        'vue_theme' => true, 
```
Then run this command again: 
```
php artisan vendor:publish --provider="Mekaeil\LaravelUserManagement\LaravelUserManagementProvider" 
```

Now follow <b> USE VUE.JS FOR YOUR PROJECT </b> section in bottom of this page.

## Important

    After vendor:publish files you should change user migration file, because we set
    mobile and email to nullable, one of them you want to set to username should not nullable in Database.
    
    ```
        $table->string('email')->nullable()->unique();
        $table->string('mobile')->nullable()->unique();
    ```

## Routes
After install package you can set this routes on your admin panel:

1. Users Management:
```
    [method type: GET, url: domain.com/admin/user-management/user ]
    admin.user_management.user.index

    [method type: GET, url: domain.com/admin/user-management/user/create ]
    admin.user_management.user.create

    [method type: POST, url: domain.com/admin/user-management/user/store ]
    admin.user_management.user.store

    [method type: GET, url: domain.com/admin/user-management/user/edit/{ID} ]
    admin.user_management.user.edit

    [method type: PUT, url: domain.com/admin/user-management/user/update/{ID} ]
    admin.user_management.user.update

    [method type: DELETE, url: domain.com/admin/user-management/user/delete/{ID} ]
    admin.user_management.user.delete

    [method type: PUT, url: domain.com/admin/user-management/user/restore/{ID} ]
    admin.user_management.user.restore

```
2. Roles Management:
```
    [method type: GET, url: domain.com/admin/user-management/role ]
    admin.user_management.role.index

    [method type: GET, url: domain.com/admin/user-management/role/create ]
    admin.user_management.role.create

    [method type: POST, url: domain.com/admin/user-management/role/store ]
    admin.user_management.role.store

    [method type: GET, url: domain.com/admin/user-management/role/edit/{ID} ]
    admin.user_management.role.edit

    [method type: PUT, url: domain.com/admin/user-management/role/update/{ID} ]
    admin.user_management.role.update

    [method type: DELETE, url: domain.com/admin/user-management/role/delete/{ID} ]
    admin.user_management.role.delete
    
```
3. Permissions Management:
```
    [method type: GET, url: domain.com/admin/user-management/permission ]
    admin.user_management.permission.index

    [method type: GET, url: domain.com/admin/user-management/permission/create ]
    admin.user_management.permission.create

    [method type: POST, url: domain.com/admin/user-management/permission/store ]
    admin.user_management.permission.store

    [method type: GET, url: domain.com/admin/user-management/permission/edit/{ID} ]
    admin.user_management.permission.edit

    [method type: PUT, url: domain.com/admin/user-management/permission/update/{ID} ]
    admin.user_management.permission.update

    [method type:DELETE, url:domain.com/admin/user-management/permission/delete/{ID} ]
    admin.user_management.permission.delete

```
4. Departments Management:
```

    [method type: GET, url: domain.com/admin/user-management/department ]
    admin.user_management.department.index

    [method type: GET, url: domain.com/admin/user-management/department/create ]
    admin.user_management.department.create

    [method type: POST, url: domain.com/admin/user-management/department/store ]
    admin.user_management.department.store

    [method type: GET, url: domain.com/admin/user-management/department/edit/{ID} ]
    admin.user_management.department.edit

    [method type: PUT, url: domain.com/admin/user-management/department/update/{ID} ]
    admin.user_management.department.update

    [method type:DELETE, url:domain.com/admin/user-management/department/delete/{ID} ]
    admin.user_management.department.delete

```
5. Authentication
```
    ****
    * IMPORTANT: THESE URL CAN BE CHANGE IN CONFIG FILE. 
    * THESE URLS ARE DEFAULT.
    ****

    [method type: GET, url: domain.com/user/login ]
    auth.user.login

    [method type: POST, url: domain.com/user/login ]
    auth.user.login

    [method type: GET, url: domain.com/user/register ]
    auth.user.register

    [method type: POST, url: domain.com/user/register ]
    auth.user.register
    
    [method type: GET, url: domain.com/user/logout ]
    auth.user.logout
```

## Demo

1. login and registration

![image](https://github.com/Mekaeil/LaravelUserManagement/blob/master/src/Public/mekaeils-package/images/login-register.jpg)

2. admin panel and create user

![image](https://github.com/Mekaeil/LaravelUserManagement/blob/master/src/Public/mekaeils-package/images/admin-panel.jpg)

<br>

![image](https://github.com/Mekaeil/LaravelUserManagement/blob/master/src/Public/mekaeils-package/images/create-user.jpg)


# USE VUE.JS FOR YOUR PROJECT

If you want to use Vue.js for your project you can use the following installation instead of the bootstrap theme.
After installing package follow this steps:
```
    1. npm install vue
    2. Add this section to your package.json file:

    "dependencies": {
        "v-tooltip": "^2.0.2",
        "vue-carousel": "^0.18.0",
        "vue-clickaway": "^2.2.2",
        "vue-lazyload": "^1.3.3",
        "vue-material": "^1.0.0-beta-11",
        "vue-router": "^3.1.3"
    }
```

Add this command in webpack file:
```
mix.js('resources/js/mekaeils-package/main.js', 'public/mekaeils-package');
```
Edit your config file:
```
        /** 
         *  IN THIS PACKAGE WE USE THE VUE.JS FOR PAGES IF YOU 
         *  WANT TO USE IT, ENABLE IT AND FOLLOW INSTALLATION STEPS IN README FILE.
         * **/       
        
        'vue_theme' => true,    //  true, false | default: false
```


## VUE JS DEMO 

1. App Vue

![image](https://github.com/Mekaeil/LaravelUserManagement/blob/master/src/Public/mekaeils-package/images/vuejs/home.jpg)

2. Login

![image](https://github.com/Mekaeil/LaravelUserManagement/blob/master/src/Public/mekaeils-package/images/vuejs/login.jpg)

3. Register

![image](https://github.com/Mekaeil/LaravelUserManagement/blob/master/src/Public/mekaeils-package/images/vuejs/register.jpg)

4. Material Kit Theme

![image](https://github.com/Mekaeil/LaravelUserManagement/blob/master/src/Public/mekaeils-package/images/vuejs/meterialKit.jpg)



#   UPDATES 

0. UPDATE PACKAGE FOR NEW VERSION OF THE LARAVEL => LARAVEL 6

1. VUE.JS FOR AUTH AND OTHER PAGES. (just vuejs theme without functinality like Auth,...)


#   IN PROGRESS 

1. Adding functionality Auth in Vuejs theme.
2. Edit structure method for API response.


## TEST
With this command you can running the test.

```
    ./vendor/bin/phpunit
```

## License

1. The LaravelUserManagement is open-source software licensed under the [MIT license](https://opensource.org/licenses/MIT).

2. Admin Template(PurpleAdmin-Free-Admin-Template) By [Bootstrap Dash](https://github.com/BootstrapDash/PurpleAdmin-Free-Admin-Template)

3. Vue Material Kit By [Creative Tim](https://www.creative-tim.com/product/vue-material-kit)
