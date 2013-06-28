#Laravel 4 Bootstrap Starter Site
`Version: 1.2.1 Stable` [![ProjectStatus](http://stillmaintained.com/andrew13/Laravel-4-Bootstrap-Starter-Site.png)](http://stillmaintained.com/andrew13/Laravel-4-Bootstrap-Starter-Site)
[![Build Status](https://api.travis-ci.org/Zizaco/confide.png)](https://travis-ci.org/andrew13/Laravel-4-Bootstrap-Starter-Site)

Laravel 4 Bootstrap Starter Site é uma simples aplicação para iniciar o desenvolvimento com Laravel 4.

Ele começou como um fork de [laravel4-starter-kit](https://github.com/brunogaspar/laravel4-starter-kit) aproveitando o starter kit alterando os módulos incluidos e também adicionando alguns. Esse é uma tradução de [laravel-4-bootstrap-starter-site](https://github.com/andrew13/Laravel-4-Bootstrap-Starter-Site).

## Características

* Twitter Bootstrap 2.3.0
* Páginas Personalizadas de Erro
	* 403 para página de acessso proibido
	* 404 para páginas não encontradas
	* 500 para erros internos no servidor
* [Confide](#confide) para Autenticação e Autorização
* Back-end (Área Administrativa)
	* Gerenciamento de usuário e função
	* Gerenciar posts e comentários
	* WYSIWYG editor para criar e editar post.
    * DataTables tabela dinâmica ordenação e filtragem.
    * Colorbox Lightbox jQuery modal popup.
* Front-end (Site/Blog)
	* Login de usuário, registro, esqueci a senha
	* Área da conta do usuário
	* Funcionalidade simples de Blog
* Packages included:
	* [Confide](#confide)
	* [Entrust](#entrust)
	* [Ardent](#ardent)
	* [Carbon](#carbon)
	* [Basset](#basset)
	* [Presenter](#presenter)
	* [Generators](#generators)

## Issues
Consulte o [github issue list](https://github.com/andrew13/Laravel-4-Bootstrap-Starter-Site/issues) para ver a lista atual.

## Wiki
[Roadmap](https://github.com/andrew13/Laravel-4-Bootstrap-Starter-Site/wiki/Roadmap)

-----

##Requesitos

	PHP >= 5.4.0 (Entrust requires 5.4, this is an increase over Laravel's 5.3.7 requirement)
	MCrypt PHP Extension

##Como instalar
### Passo 1: Obter o código
#### Opção 1: Git Clone

	git clone git://github.com/andrew13/Laravel-4-Bootstrap-Starter-Site.git laravel

#### Opção 2: Faça o download do repositório

    https://github.com/andrew13/Laravel-4-Bootstrap-Starter-Site/archive/master.zip

### Passo 2: Use Composer para instalar dependências
#### Opção 1: Composer não está instalado globalmente

    cd laravel
	curl -s http://getcomposer.org/installer | php
	php composer.phar install --dev

#### Opção 2: Composer está instalado globalmente

    cd laravel
	composer install --dev

Se você não tiver, pode querer fazer [composer ser instalado globalmente](http://andrewelkins.com/programming/php/setting-up-composer-globally-for-laravel-4/) para futura facilidade de uso.

Por favor, note o uso da flag `--dev`.

Alguns pacotes usados para pré-processar e minificar os assets são requeridos no ambiente de desenvolvimento.

Quando você implementar seu projeto em um ambiente de produção, você vai querer fazer o upload do arquivo ***composer.lock*** usado no ambiente de desenvolvimento e só executar `php composer.phar ìnstall` no servidor de produção.

Isso vai ìgnorar os pacotes de desenvolvimento e assegurar a versão dos pacotes instalados no servidor de produção coincidem com aqueles que você desenvolveu.

NUNCA executar `php composer.phar update` no seu servidor de produção.

### Passo 3: Configuração dos Ambientes

Laravel 4 irá carregar os arquivos de configuração, dependendo do seu ambiente. Basset também irá construir coleções, dependendo da configuração do ambiente.

Abra ***bootstrap/start.php*** e edite as seguintes linhas de acordo com suas configurações. Você quiser utilizar o nome da máquina no Windows e seu hostname no Mac OS X e Linux (tipo `hostname` no terminal). Usando o nome da máquina vai permitir que o comando `php artisan` para usar os arquivos de configuração do direito também.

    $env = $app->detectEnvironment(array(

        'local' => array('your-local-machine-name'),
        'staging' => array('your-staging-machine-name'),
        'production' => array('your-production-machine-name'),

    ));

Agora crie uma pasta dentro de ***app/config*** que corresponde ao seu ambiente que o código é implementado. Isso provavelmente será ***local*** quando você primeiro inicia um projeto.

Agora vai ser copiar o arquivo de configuração inicial dentro desta pasta antes de editá-lo. Vamos começar com a ***app/config/app.php***. Então ***app/config/local/app.php*** provavelmente será algo parecido com isso, como o resto da configuração pode ser deixado para os padrões do arquivo de configuração inicial:

    <?php

    return array(

        'url' => 'http://myproject.local',

        'timezone' => 'UTC',

        'key' => 'YourSecretKey!!!',

        'providers' => array(
        /* Descomentar para usar no desenvolvimento */
            'Way\Generators\GeneratorsServiceProvider', // Generators
            'Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider', // IDE Helpers

        ),

    );

### Step 4: Configuração do banco de dados

Agora que você tem o ambiente configurado, você precisa criar um configuração de banco de dados para ele. Copie o arquivo ***app/config/database.php*** para ***app/config/local*** e edite para corresponder às suas configutações de baco de dados. Você pode remover todas as partes que não foram alteradas, como este arquivo de configuração será carregado em relação ao incial.

### Passo 5: Configuração de E-mail

Do mesmo mode, copie o arquivo de configuração ***app/config/mail.php*** pra ***app/config/local/mail.php***. Agora defina o `address` e `name` de `from` array em ***config/mail.php***. Esses serão usado para enviar e-mails de confirmação de conta e de redefinição para os usuários. Se você não definir que o registro irá falhar porque ele não pode enviar um e-mail de confirmação.

### Passo 6: Popular banco de dados
Rode estes comandos para criar e popular a tabela Users:

	php artisan migrate
	php artisan db:seed

### Passo 7: Definir chave de criptografia
Em ***app/config/app.php***

```
/*
|--------------------------------------------------------------------------
| Encryption Key
|--------------------------------------------------------------------------
|
| This key is used by the Illuminate encrypter service and should be set
| to a random, long string, otherwise these encrypted values will not
| be safe. Make sure to change it before deploying any application!
|
*/
```

	'key' => 'YourSecretKey!!!',

Você pode usar `artisan` para fazer isso

    php artisan key:generate

Uma vez que você gerou sua chave, você pode querer copiá-lo para o seu arquivo de configuração local ***app/config/local/app.php***  ter uma chave de criptografia diferente para cada ambiente. Uma pequena dica, reverter a chave de volta para ***'YourSecretKey!'*** em ***app/config/app.php*** uma vez que você copiou. Agora ele pode ser gerado novamente quando você mover o projeto para outro ambiente.

### Passo 8: Certifique-se que app;storage é gravavel pelo servidor.

Se as permissões estão configuradas corretamente:

    chmod -R 775 app/storage

Deve funcionar, se não, tentar

    chmod -R 777 app/storage

### Passo 9: Build Assets

If you have setup your environments, basset will know you are in development and will build the assets automatically and will not apply certain filters such as minification or combination to keep the code readable. You will need to make the folder where the assets are built writable:

If permissions are set correctly:

    chmod -R 775 public/assets/compiled

Should work, if not try

    chmod -R 777 public/assets/compiled

To force a build of the dev collection use:

```
php artisan basset:build
```

The starter site uses two asset collections, ***public*** and ***admin***. While in development, assets will be built in two folders, ***public*** and ***admin***, inside of ***public/assets/compiled***. These are ignored by git as you do not want them on your production server. Once you are ready to push or upload the code to production run:

```
php artisan basset:build -p public
php artisan basset:build -p admin
```

This will build the production assets in ***public/assets/compiled*** which will be versioned in git and should be uploaded to your production server.

### Passo 10: Start Page

### User login with commenting permission
Navigate to your Laravel 4 website and login at /user/login:

    username : user
    password : user

Create a new user at /user/create

### Admin login
Navigate to /admin

    username: admin
    password: admin

-----
## Application Structure

The structure of this starter site is the same as default Laravel 4 with one exception.
This starter site adds a `library` folder. Which, houses application specific library files.
The files within library could also be handled within a composer package, but is included here as an example.

### Development

For ease of development you'll want to enable a couple useful packages. This requires editing the `app/config/app.php` file.

```
    'providers' => array(

        [...]

        /* Uncomment for use in development */
//        'Way\Generators\GeneratorsServiceProvider', // Generators
//        'Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider', // IDE Helpers

    ),
```
Uncomment the Generators and IDE Helpers. Then you'll want to run a composer update with the dev flag.

```
php composer.phar update
```
This adds the generators and ide helpers.
To make it build the ide helpers automatically you'll want to modify the post-update-cmd in `composer.json`

```
		"post-update-cmd": [
			"php artisan ide-helper:generate",
			"php artisan optimize"
		]
```

### Production Launch

By default debugging is enabled. Before you go to production you should disable debugging in `app/config/app.php`

```
    /*
    |--------------------------------------------------------------------------
    | Application Debug Mode
    |--------------------------------------------------------------------------
    |
    | When your application is in debug mode, detailed error messages with
    | stack traces will be shown on every error that occurs within your
    | application. If disabled, a simple generic error page is shown.
    |
    */

    'debug' => false,
```

## Troubleshooting

### Styles are not displaying

You may need to recompile the assets for basset. This is easy to with one command.

```
php artisan basset:build
```

-----
## Included Package Information
<a name="confide"></a>
## Confide Authentication Solution

Used for the user auth and registration. In general for user controllers you'll want to use something like the following:

    <?php

    use Zizaco\Confide\ConfideUser;

    class User extends ConfideUser {

    }

For full usage see [Zizaco/Confide Documentation](https://github.com/zizaco/confide)

<a name="entrust"></a>
## Entrust Role Solution

Entrust provides a flexible way to add Role-based Permissions to Laravel4.

    <?php

    use Zizaco\Entrust\EntrustRole;

    class Role extends EntrustRole
    {

    }

For full usage see [Zizaco/Entrust Documentation](https://github.com/zizaco/entrust)

<a name="ardent"></a>
## Ardent - Used for handling repetitive validation tasks.

Self-validating, secure and smart models for Laravel 4's Eloquent ORM

For full usage see [Ardent Documentation](https://github.com/laravelbook/ardent)

<a name="carbon"></a>
## Carbon

A fluent extension to PHPs DateTime class.

```php
<?php
printf("Right now is %s", Carbon::now()->toDateTimeString());
printf("Right now in Vancouver is %s", Carbon::now('America/Vancouver'));  //implicit __toString()
$tomorrow = Carbon::now()->addDay();
$lastWeek = Carbon::now()->subWeek();
$nextSummerOlympics = Carbon::createFromDate(2012)->addYears(4);

$officialDate = Carbon::now()->toRFC2822String();

$howOldAmI = Carbon::createFromDate(1975, 5, 21)->age;

$noonTodayLondonTime = Carbon::createFromTime(12, 0, 0, 'Europe/London');

$worldWillEnd = Carbon::createFromDate(2012, 12, 21, 'GMT');
```

For full usage see [Carbon](https://github.com/briannesbitt/Carbon)

<a name="basset"></a>
## Basset

A Better Asset Management package for Laravel.

Adding assets in the configuration file `config/packages/jasonlewis/basset/config.php`
```php
'collections' => array(
        'public-css' => function($collection)
        {
            $collection->add('assets/css/bootstrap.min.css');
            $collection->add('assets/css/bootstrap-responsive.min.css');
        },
    ),
```

Compiling assets

    $ php artisan basset:build

I would recommend using development collections for development instead of compiling .

For full usage see [Using Basset by Jason Lewis](http://jasonlewis.me/code/basset/4.0)

<a name="presenter"></a>
## Presenter

Simple presenter to wrap and render objects. Think of it of a way to modify an asset for the view layer only.
Control the presentation in the presentation layer not in the model.

The core idea is the relationship between two classes: your model full of data and a presenter which works as a sort of wrapper to help with your views.
For instance, if you have a `User` object you might have a `UserPresenter` presenter to go with it. To use it all you do is `$userObject = new UserPresenter($userObject);`.
The `$userObject` will function the same unless a method is called that is a member of the `UserPresenter`. Another way to think of it is that any call that doesn't exist in the `UserPresenter` falls through to the original object.

For full usage see [Presenter Readme](https://github.com/robclancy/presenter)

<a name="generators"></a>
## Laravel 4 Generators

Laravel 4 Generators package provides a variety of generators to speed up your development process. These generators include:

- `generate:model`
- `generate:seed`
- `generate:test`
- `generate:view`
- `generate:migration`
- `generate:resource`
- `generate:scaffold`
- `generate:form`
- `generate:test`

For full usage see [Laravel 4 Generators Readme](https://github.com/JeffreyWay/Laravel-4-Generators/blob/master/readme.md)


-----
## License

This is free software distributed under the terms of the MIT license

## Additional information

Inspired by and based on [laravel4-starter-kit](https://github.com/brunogaspar/laravel4-starter-kit)

Any questions, feel free to [contact me](http://twitter.com/andrewelkins).
