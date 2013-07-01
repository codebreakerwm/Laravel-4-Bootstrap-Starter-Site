#Laravel 4 Bootstrap Starter Site
`Version: 1.2.1 Stable` [![ProjectStatus](http://stillmaintained.com/andrew13/Laravel-4-Bootstrap-Starter-Site.png)](http://stillmaintained.com/andrew13/Laravel-4-Bootstrap-Starter-Site)
[![Build Status](https://api.travis-ci.org/Zizaco/confide.png)](https://travis-ci.org/andrew13/Laravel-4-Bootstrap-Starter-Site)

***Laravel 4 Bootstrap Starter Site*** é uma simples aplicação para iniciar o desenvolvimento com Laravel 4.

Ele começou como um fork de [laravel4-starter-kit](https://github.com/brunogaspar/laravel4-starter-kit) aproveitando o starter kit alterando os módulos incluidos e também adicionando alguns. 

Original [laravel-4-bootstrap-starter-site](https://github.com/andrew13/Laravel-4-Bootstrap-Starter-Site) em inglês.

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
* Pacotes incluídos:
	* [Confide](#confide)
	* [Entrust](#entrust)
	* [Ardent](#ardent)
	* [Carbon](#carbon)
	* [Basset](#basset)
	* [Presenter](#presenter)
	* [Generators](#generators)

## Issues
Consulte a [lista de issues no github](https://github.com/andrew13/Laravel-4-Bootstrap-Starter-Site/issues) para ver a lista atual.

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

### Passo 2: Use o Composer para instalar as dependências
#### Opção 1: Composer não está instalado globalmente

    cd laravel
	curl -s http://getcomposer.org/installer | php
	php composer.phar install --dev

#### Opção 2: Composer está instalado globalmente

    cd laravel
	composer install --dev

Se você não tiver, pode querer deixar o [composer ser instalado globalmente](http://andrewelkins.com/programming/php/setting-up-composer-globally-for-laravel-4/) para futura facilidade de uso.

Por favor, note o uso da flag `--dev`.

Alguns pacotes usados para pré-processar e minificar os assets são requeridos no ambiente de desenvolvimento.

Quando você implementar seu projeto em um ambiente de produção, você vai querer fazer o upload do arquivo ***composer.lock*** usado no ambiente de desenvolvimento e só executar `php composer.phar ìnstall` no servidor de produção.

Isso vai ìgnorar os pacotes de desenvolvimento e assegurar que a versão dos pacotes instalados no servidor de produção coincidem com aqueles que você desenvolveu.

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

Deve funcionar, se não tentar

    chmod -R 777 app/storage

### Passo 9: Construindo os Assets

Se você configurar seus ambientes, Basset irá saber que você está em desenvolvimento e irá construir os assets automaticamente e não irá aplicar determinados filtros, como minificação ou combinaçãço para manter o código legível. Você precisará deixar a pasta dos assets com permissão de escrita.

Se as permissões estão configuradas corretamente:

    chmod -R 775 public/assets/compiled

Deve funcionar, se não tentar

    chmod -R 777 public/assets/compiled

Para forçar uma contrução da coleção dev usar:

```
php artisan basset:build
```

O starter site usa duas coleções de assets, ***public*** e ***admin***. Enquando em desenvolvimento, os assets irá estar em duas pastas, ***public*** e ***admin***, dentro de ***public/assets/compiled***. Estes são ignorados pelo git, como você não quer eles em seu servidor de produção. Uma vez que você esteja pronto para enviar seu código para produção, executar:

```
php artisan basset:build -p public
php artisan basset:build -p admin
```

Isto irá construir os assets de produção em ***public/assets/compiled*** que serão versionados no git e deve ser enviados para seu servidor de produção.

### Passo 10: Página Inicial

### Acesso de usuário com permissão para comentar
Navegue no seu website Laravel 4 e acesse /user/login:

    nome de usuário : user
    senha : user


Criar um novo usuário em /user/create

### Acesso de Admin
Navegue para /admin

    nome de usuário: admin
    senha: admin

-----
## Estrutura da aplicação

A estrutura desse startes site é o mesmo que o padrão Laravel 4 com uma exceção.
Este starter site adiciona uma pasta `library`. Que, especifica o local dos arquivos da biblioteca.
Os arquivos dentro de library também pode ser manipulado dentro do pacote do composer, mas é incluido aqui como um exemplo.

### Desenvolvimento

Para facilidade de desenvolvimento, precisará habilitar alguns pacotes úteis. Isto requer a edição do arquivo `app/config/app.php`.

```
    'providers' => array(

        [...]

        /* Uncomment for use in development */
//        'Way\Generators\GeneratorsServiceProvider', // Generators
//        'Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider', // IDE Helpers

    ),
```
Descomente os Generators e IDE Helpers. Depois irá executar o composer para atualizar com a dev flag.

```
php composer.phar update
```
Isso adiciona os generators e ide helpers.
Para criar os ide helpers automaticamente, precisará modifigcar o post-update-cmd em `composer.json`.

```
		"post-update-cmd": [
			"php artisan ide-helper:generate",
			"php artisan optimize"
		]
```

### Colocando em Produção

Por padrão, depuração é habilidato. Antes de ir para produçãço, você deve desabilitar a depuração em `app/config/app.php`

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

## Solução de Problemas

### Estilos não estão exibindo

Você pode precisar recompilar os assets para basset. Isto é fácil com o comando:

```
php artisan basset:build
```

-----
## Informações de Pacotes Incluídos
<a name="confide"></a>
## Confide Solução de Autenticação

Usar para a autenticação e registro de usuário. Em geral, para os controladores de usuário que irá querer usar algo como o seguinte:

    <?php

    use Zizaco\Confide\ConfideUser;

    class User extends ConfideUser {

    }

Para uso completo consulte [Zizaco/Confide Documentação](https://github.com/zizaco/confide)

<a name="entrust"></a>
## Entrust Solução de Função

Entrust provê uma modo flexivel para adicionar permissão baseado em funções para Laravel4.

    <?php

    use Zizaco\Entrust\EntrustRole;

    class Role extends EntrustRole
    {

    }

Para uso completo consulte [Zizaco/Entrust Documentação](https://github.com/zizaco/entrust)

<a name="ardent"></a>
## Ardent - Usado para lidar com tarefas repetidas de validação.

Modelos de auto-validação, seguro e inteligente para Eloquent ORM de Laravel 4.

Para uso completo consulte [Ardent Documentação](https://github.com/laravelbook/ardent)

<a name="carbon"></a>
## Carbon

Uma extensão fluente para classe DateTime do PHP.


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
?>
```

Para uso completo consulte [Carbon](https://github.com/briannesbitt/Carbon)

<a name="basset"></a>
## Basset

Um pacote melhor de gerenciamento de Asset para Laravel.

Adicionando assets no arquivo de configuração `config/packages/jasonlewis/basset/config.php`

```php
'collections' => array(
        'public-css' => function($collection)
        {
            $collection->add('assets/css/bootstrap.min.css');
            $collection->add('assets/css/bootstrap-responsive.min.css');
        },
    ),
```

Compilando assets

    $ php artisan basset:build

Eu recomendo usar coleções de desenvolvimento para o desenvolvimento em vez de compilar.

Para uso completo consulte [Using Basset by Jason Lewis](http://jasonlewis.me/code/basset/4.0)

<a name="presenter"></a>
## Presenter

Simples apresentador para cobrir e renderizar objetos. Pense disso de um modo a alterar um asset somente para a camada de visão.
Controle a apresentação na camada de apresentação, não no modelo.

A ideia básica é a relacão entre duas classes: seu modelo completo de dados e um aprensetador que trabalha como uma espécie de invólucro para ajudar com suas exibições.
Por exemplo, se você tiver um objeto `User` você pode ter um aprensentador `UserPresenter` para ir com ele. Para usar, tudo que você tem que fazer é `$userObject = new UserPresenter($userObject);`.
O `$userObject` funcionará da mesma forma, a menos que que um método chamado é um membro de `UserPresenter`. Outro modo de pensar sobre isso é que qualquer chamada que não exista no `UserPresenter` cai até o objeto original.

Para uso completo consulte [Presenter Readme](https://github.com/robclancy/presenter)

<a name="generators"></a>
## Laravel 4 Generators

O pacote de geradores do Laravel 4 provê uma variedade de geradores para acelerar o processo de desenvolvimento. Estes geradores incluem:

- `generate:model`
- `generate:seed`
- `generate:test`
- `generate:view`
- `generate:migration`
- `generate:resource`
- `generate:scaffold`
- `generate:form`
- `generate:test`

Para uso completo consulte [Laravel 4 Generators Readme](https://github.com/JeffreyWay/Laravel-4-Generators/blob/master/readme.md)

-----
## Licença

Este é um software livre distribuído sob os termos da licença MIT

## Informações Adicionais

Inspirado e baseado em [laravel4-starter-kit](https://github.com/brunogaspar/laravel4-starter-kit)

Qualquer dúvida, fique livre em [contactar-me](http://twitter.com/andrewelkins).
