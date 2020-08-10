# Making Nova Tab Translatable

This package contains a `NovaTabTranslatable` class you can use to make any Nova field type translatable with tabs.

Imagine you have this `fields` method in a Post Nova resource:

```php
public function fields(Request $request)
{
    return [
        ID::make()->sortable(),

        NovaTabTranslatable::make([
            SluggableText::make('Title')->slug('Slug'),
            Slug::make('Slug')->readonly(),
            Trix::make('text'),
            Text::make('Keywords'),
            Text::make('Description'),
        ]),
    ];
}
```

That Post Nova resource will be rendered like this.

![screenshot](https://kongulov.github.io/sitescreenshots/kongulov_nova-tab-translatable_1.png?v=4)
![screenshot](https://kongulov.github.io/sitescreenshots/kongulov_nova-tab-translatable_2.png?v=4)

## Requirements

- `laravel/nova: ^2.9`
- `spatie/laravel-translatable: ^4.0`

## Installation

Install the package in a Laravel Nova project via Composer:

```bash
# Install nova-tab-translatable
composer require kongulov/nova-tab-translatable

# Publish configuration
php artisan vendor:publish --tag="nova-tab-translatable-config"
```

This is the contents of the file which will be published at `config/nova_tab_translatable.php`
```php
<?php

return [
    /*
     * The source of supported locales on the application
     * Available selection "array", "database". Default array
     */
    'source' => 'array',

    /*
     * If you choose array selection, you should add all supported translation on it as "code"
     */
    'locales' => [
        'en', 'fr', 'es'
    ],

    /*
     * If you choose database selection, you should choose the model responsible for retrieving supported translations
     * And choose the 'code_field' for example "en", "fr", "es"...
     */
    'database' => [
        'model' => 'App\\Language',
        'code_field' => 'lang',
    ],
];
```

## Usage

You must prepare your model [as explained](https://github.com/spatie/laravel-translatable#making-a-model-translatable) in the readme of laravel-translatable. In short: you must add `json` columns to your model's table for each field you want to translate. Your model must use the `Spatie\Translatable\HasTranslations` on your model. Finally, you must also add a `$translatable` property on your model that holds an array with the translatable attribute names.

Now that your model is configured for translations, you can use `NovaTabTranslatable` in the related Nova resource. Any fields you want to display in a multilingual way can be passed as an array to `NovaTabTranslatable`. 

```php
public function fields(Request $request)
{
    return [
        ID::make()->sortable(),

        NovaTabTranslatable::make([
            SluggableText::make('Title')->slug('Slug'),
            Slug::make('Slug')->readonly(),
            Trix::make('text'),
            Text::make('Keywords'),
            Text::make('Description'),
        ]),
    ];
}
```

## Credits

- [Ramiz Kongulov](https://github.com/kongulov)

## License

This project is open-sourced software licensed under the [MIT license](LICENSE.md).