title: Symfony2 i18n best practice and tips
date: 2016-01-14 22:19:08
tags: symfony2
categories: symfony2
toc: true
---

This article summaried the my know-how about doing i18n in symfony2 project. There are some essential packages and bundles, which I used in my system to apply i18n easier. We have to installed them into your symfony 2 project at first. Looking only in templates for strings to translate is wrong – with Symfony2 they can come from a hell lot of places. That’s part of what `JMSTranslationBundle` tries to solve with extractors for:

- all calls to the `trans`, or `transChoice` method within PHP files
- all classes implementing the 'TranslationContainerInterface'
- all form labels that are defined as options to the ->add() 
method of `FormBuilder`
- messages declared in validation constraints

## Set Up

### composer.json
add the following packages to `composer.json`
```json
        "twig/extensions": "^1.3",
        "jms/translation-bundle": "~1.1.0",
        "jms/di-extra-bundle": "dev-master"
```

### AppKernel.php
register the two bundles for non-production staging: `dev` & `test`
add the following code into `/app/AppKernel.php`

```php
        if (in_array($this->getEnvironment(), array('dev', 'test'))) {
            $bundles[] = new \JMS\TranslationBundle\JMSTranslationBundle();
            $bundles[] = new \JMS\DiExtraBundle\JMSDiExtraBundle($this);
            $bundles[] = new \JMS\AopBundle\JMSAopBundle();
        }
```

### service.yml
added `Twig_Extensions_Extension_Text` as new service for using web-based translation tool

```yml
services:
    twig.text_extension:
            class: Twig_Extensions_Extension_Text
            tags:
                - name: twig.extension
```

### config.yml
add this config code to `config.yml` to config `jms_translation` bundle

```yml
jms_translation:
    configs:
        app:
            dirs: [ %kernel.root_dir%/../src]
            output_dir: %kernel.root_dir%/Resources/translations
            ignored_domains: [routes]
            excluded_names: ["*TestCase.php", "*Test.php"]
            excluded_dirs: [cache, data, logs]
```

### routing_dev.yml
add routing config code to `app/config/routing_dev.yml`, in order to access translation tool in browser via `http://servername.dev/app_dev/_trans`

```yml
_main:
    resource: routing.yml

JMSTranslationBundle_ui:
    resource: @JMSTranslationBundle/Controller/
    type:     annotation
    prefix:   /_trans
```

### update composer
```bash
composer update
```

## Translations in twig

### trans in *.html.twig
use the following translation keys and filters to make translations in `*.html.twig`. Theses examples will use predefined service `twig.text_extension`. More information about twig extension, see http://twig.sensiolabs.org/documentation#reference

> If you want to define specific domain, so that a specific translation xlf file will be generated, please use `trans_default_domain` in each twig script

```twig
{% trans_default_domain "useraddresses" %}
```


```twig
{{ "cart_thead_title"|trans|upper }}
 
{{ "cart_thead_decription"|trans|capitalize }}

{{ "cart_thead_price"|trans|capitalize }}

{{ "cart_thead_price"|trans|wordwrap(10) }} {# mac 10 characters reach line #}

{{ "cart_thead_price"|trans|truncate(10) }} {# cut the first 10 Chars #}
```


### trans in Javascript

see more about translation in javascript https://github.com/willdurand/BazingaJsTranslationBundle/blob/master/Resources/doc/index.md

## Generate xliff from twig(src)
generate(update) all translations and extract to centralized folder `app/Resources/translations`

```bash
$ php app/console translation:extract de --dir=./src/ --output-dir=./app/Resources/translations
```

generate(update) translations for specified bundle

```bash
$ php app/console translation:extract de --bundle=AppBundle
```


## Translate messages via web tool
after generating all translations, you can translate the language messages via WEB GUI `http://localhost:8080/app_dev.php/_trans/`

{% asset_img jms_translation_gui.png %}
