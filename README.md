## Laravel 5 Google Forms (using Google Scripts) - Pull requests are welcome!

Installation
------------

Installation using composer:

```
composer require akaramires/laravel-googleforms
```

And add the service provider in config/app.php:

```php
Akaramires\GoogleForms\GoogleFormsServiceProvider::class,
```

Configuration
-------------

### Laravel

Laravel 5 Google Forms requires configuration.

To get started, first publish the package config file:

```
php artisan vendor:publish --provider="Akaramires\GoogleForms\GoogleFormsServiceProvider"
```

You need also get access token

```
php artisan google-forms:token
```

### Google Scripts

* Go to https://script.google.com
* Create new project
* Create 5 scripts (content of them you can find in `vendor/akaramires/laravel-googleforms/scripts` directory)
* Click on Publish->Deploy as API executable
* Copy API ID and paste into `config/googleforms.php` 

Examples
--------

### Simple form

```php
$form = App::make('GoogleForm');

$params = [
    'file_name'                   => sha1(microtime(true)),
    'file_description'            => 'File was created by ' . get_current_user() . ' (' . app()->environment() . ')',
    'title'                       => 'Super title',
    'description'                 => 'Super description',
    'confirmation_message'        => 'Your response has been recorded.',
    'show_progress'               => false,
    'limit_one_response_per_user' => false,
    'shuffle_questions'           => false,
    'show_link_to_respond_again'  => false,
    'publish_and_show'            => false,
    'allow_response_edits'        => false,
    'accepting_responses'         => true,
];

$form->createForm($params);
```

### Form with fields

```php
$fields = [];

$textItem = new TextItem();
// $textItem->setId($field->google_id); # if you have saved field id
$textItem->setTitle('Field title');
$textItem->setHelpText('Field help text');
$textItem->setRequired(true);
$textItem->setIndex(0);

$fields[] = $form->createOrUpdateElement($textItem, true);

$queries = [
    'form'   => $form->createForm($params, true),
    'fields' => $fields,
];

$request = $this->form->batchRequest($queries);;

print_r($request);
```
