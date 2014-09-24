# jwt-auth

> JSON Web Token Authentication for Laravel

[![Build Status](http://img.shields.io/travis/tymondesigns/jwt-auth.svg?style=flat-square)](https://travis-ci.org/tymondesigns/jwt-auth)
[![Scrutinizer Code Quality](http://img.shields.io/scrutinizer/g/tymondesigns/jwt-auth.svg?style=flat-square)](https://scrutinizer-ci.com/g/tymondesigns/jwt-auth/?branch=master)

#### *Docs work in progress!*

## Installation

Install via composer - edit your `composer.json` to require the package.

```js
"require": {
    "tymon/jwt-auth": "0.*"
}
```

Then run `composer update` in your terminal to pull it in.

Once this has finished, you will need to add the service provider to the `providers` array in `app/config/app.php` as follows:

```php
'Tymon\JWTAuth\JWTAuthServiceProvider'
```

Next, also in the `app/config/app.php` file, under the `aliases` array, you may want to add the `JWTAuth` facade.

```php
'JWTAuth' => 'Tymon\JWTAuth\Facades\JWTAuth'
```

Finally, you will want to publish the config using the following command:

```bash
$ php artisan config:publish tymon/jwt-auth
```

##### **Don't forget to set a secret key in the config file!**

And you're done!

## Basic Usage

### Creating Tokens

There are a number of ways you can generate a token. The usual flow would be to pass some credentials and the package will try to authenticate the user and return a fully formed JSON Web Token.

##### Creating a token based on user's credentials

```php
// simple example
Route::post('auth/login', function () {
    $credentials = Input::only('email', 'password');
    
    if ( ! $token = JWTAuth::attempt($credentials) )
    {
        // return 401 error response
    }
    
    return Response::json(compact('token'));
});
```

##### Creating a token based on a User object

```php
$user = User::find(1);
$token = JWTAuth::fromUser($user);
```

### Retrieving User from a token

Once a user has "logged in" (e.g. provided their credentials via a login form) to your application, then the next step would be to make a subsequent request, with the token, to retrieve the users' details, so you can show them as being logged in.

```php
// simple example
Route::post('me', ['before' => 'jwt-auth', function () {

    $token = Input::get('token');
    
    if ( ! $user = JWTAuth::toUser($token) )
    {
        // user not found
    }
    
    return Response::json(compact('user'));
}]);

```
