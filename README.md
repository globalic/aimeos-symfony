Aimeos Symfony2 bundle
======================

Aimeos Symfony2 web shop bundle

## Installation

Add the main bundle class to the `registerBundles()` method in the `app/AppKernel.php` file:

```
    $bundles = array(
        new Aimeos\ShopBundle\AimeosShopBundle(),
        ...
    );
```

Make sure that the database is set up and it is configured in your config.yml. Then add these lines to your composer.json of your Symfony2 project:

```
    "repositories": [ {
        "type": "vcs",
        "url": "https://github.com/nsendetzky/arcavias-core"
    } ],
    "minimum-stability": "dev",
    "require": {
        "aimeos/symfony2-bundle": "dev-master",
        ...
    },
    "scripts": {
        "post-install-cmd": [
            "Aimeos\\ShopBundle\\Composer\\ScriptHandler::installBundle",
            "Aimeos\\ShopBundle\\Composer\\ScriptHandler::updateDatabase",
            ...
        ],
        "post-update-cmd": [
            "Aimeos\\ShopBundle\\Composer\\ScriptHandler::installBundle",
            "Aimeos\\ShopBundle\\Composer\\ScriptHandler::updateDatabase",
            ...
        ]
    }
```

Afterwards, install the Aimeos shop bundle using `composer update`

The last step executed in the directory of your Symfony2 application installs the assets (CSS, Javascript, images, etc.) of the bundles into the web/bundles/ directory so they are publically available:

`php app/console assets:install`


## Usage

In your `app/config/routing.yml` file you need to add these lines to add the pre-defined routes from the Aimeos shop bundle:

```
aimeos_shop:
    resource: "@AimeosShopBundle/Resources/config/routing.yml"
    prefix:   /
```

To see all components and get everything working, you also need to adapt your Twig base template in `app/Resources/view`. This is a working example using the [Twitter bootstrap CSS framework](http://getbootstrap.com/getting-started/#download):

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
{% block header %}{% endblock %}
        <link rel="icon" type="image/x-icon" href="{{ asset('favicon.ico') }}" />
        <link href="{{ asset('css/bootstrap.min.css') }}" rel="stylesheet">
        <title>{% block title %}Aimeos shop{% endblock %}</title>
{% block stylesheets %}{% endblock %}
    </head>
    <body>
        <div class="navbar navbar-static" role="navigation">
{% block head %}{% endblock %}
        </div>
{% if block('nav') %}
        <div class="col-xs-12 col-sm-3">
            {% block nav %}{% endblock %}
        </div>
{% endif %}
{% if block('nav') or block('aside') %}
        <div class="col-xs-12 col-sm-9">
{% else %}
        <div class="col-xs-12">
{% endif %}
            {% block stage %}{% endblock %}
            {% block body %}{% endblock %}
        </div>
{% if block('aside') %}
        <div class="col-xs-12 col-sm-3">
            {% block aside %}{% endblock %}
        </div>
{% endif %}
{% block javascripts %}{% endblock %}
        <script src="{{ asset('js/bootstrap.min.js') }}"></script>
    </body>
</html>
```

Copy the

* css/
* js/
* fonts/

directories from the bootstrap .zip package into the web/ directory of your Symfony2 application. Then, you should be able to call the catalog list page in your browser using

```http://<your web root>/app.php/list```
