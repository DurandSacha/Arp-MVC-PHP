# Arp MVC PHP framework

Lightweight and simple framework for PHP developers [open-source](LICENSE).

Arp exists to provide a lightweight architecture and framework, useful for small applications where large frameworks are not needed. Quickly start your application with Arp

## Start

1. Clone the project
2. run **composer install** .
3. Open [Src/Config.php](Src/Config.php) and enter your database configuration data.
4. Create routes, add controllers, views and models.

## Configuration

Configuration settings are stored in the [Src/Config.php](Src/Config.php) class. Default settings include database connection data and a setting to show or hide error detail. You can access the settings in your code like this: `Config::DB_HOST`. You can add your own configuration settings in here.

## Routing

The [Router](Core/Router.php) translates URLs into controllers and actions. Routes are added in the [front controller](public/index.php). A sample home route is included that routes to the `index` action in the [Home controller](Src/Controllers/Home.php).

Routes are added with the `add` method. You can add fixed URL routes, and specify the controller and action, like this:

```php
$router->add('', ['controller' => 'Home', 'action' => 'index']);
$router->add('posts/index', ['controller' => 'Posts', 'action' => 'index']);
```

Or you can add route **variables**, like this:

```php
$router->add('{controller}/{action}');
```

In addition to the **controller** and **action**, you can specify any parameter you like within curly braces, and also specify a custom regular expression for that parameter:

```php
$router->add('{controller}/{id:\d+}/{action}');
```

You can also specify a namespace for the controller:

```php
$router->add('admin/{controller}/{action}', ['namespace' => 'Admin']);
```

## Controllers

Controllers respond to user actions (clicking on a link, submitting a form etc.). Controllers are classes that extend the [Core\Controller](Core/Controller.php) class.

Controllers are stored in the `Src/Controllers` folder. A sample [Home controller](Src/Controllers/Home.php) included. Controller classes need to be in the `Src/Controllers` namespace. You can add subdirectories to organise your controllers, so when adding a route for these controllers you need to specify the namespace (see the routing section above).

Controller classes contain methods that are the actions. To create an action, add the **`Action`** suffix to the method name. The sample controller in [Src/Controllers/Home.php](Src/Controllers/Home.php) has a sample `index` action.

You can access route parameters (for example the **id** parameter shown in the route examples above) in actions via the `$this->route_params` property.

## Views

Views are used to display information (normally HTML). View files go in the `Src/Views` folder. Views can be in one of two formats: standard PHP, but with just enough PHP to show the data. No database access or anything like that should occur in a view file. You can render a standard PHP view in a controller, optionally passing in variables, like this:

```php
View::render('Home/index.php', [
    'name'    => 'Dave',
    'colours' => ['red', 'green', 'blue']
]);
```

The second format uses the [Twig](http://twig.sensiolabs.org/) templating engine. Using Twig allows you to have simpler, safer templates that can take advantage of things like [template inheritance](http://twig.sensiolabs.org/doc/templates.html#template-inheritance). You can render a Twig template like this:

```php
View::renderTemplate('Home/index.html', [
    'name'    => 'Dave',
    'colours' => ['red', 'green', 'blue']
]);
```

A sample Twig template is included in [Src/Views/Home/index.html](Src/Views/Home/index.html) that inherits from the base template in [Src/Views/base.html](Src/Views/base.html).

## Models

Models are used to get and store data in your Srclication. They know nothing about how this data is to be presented in the views. Models extend the `Core\Model` class and use [PDO](http://php.net/manual/en/book.pdo.php) to access the database. They're stored in the `Src/Models` folder. A sample user model class is included in [Src/Models/User.php](Src/Models/User.php). You can get the PDO database connection instance like this:

```php
$db = static::getDB();
```

## Errors

If the `SHOW_ERRORS` configuration setting is set to `true`, full error detail will be shown in the browser if an error or exception occurs. If it's set to `false`, a generic message will be shown using the [Src/Views/404.html](Src/Views/404.html) or [Src/Views/500.html](Src/Views/500.html) views, depending on the error.

