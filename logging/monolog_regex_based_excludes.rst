.. index::
   single: Logging
   single: Logging; Exclude 404 Errors
   single: Monolog; Exclude 404 Errors

How to Configure Monolog to Exclude 404 Errors from the Log
===========================================================

.. tip::

    Read :doc:`/logging/monolog_exclude_http_codes` to learn about a similar
    but more generic feature that allows to exclude logs for any HTTP status
    code and not only 404 errors.

Sometimes your logs become flooded with unwanted 404 HTTP errors, for example,
when an attacker scans your app for some well-known application paths (e.g.
`/phpmyadmin`). When using a ``fingers_crossed`` handler, you can exclude
logging these 404 errors based on a regular expression in the MonologBundle
configuration:

.. configuration-block::

    .. code-block:: yaml

        # config/packages/prod/monolog.yaml
        monolog:
            handlers:
                main:
                    # ...
                    type: fingers_crossed
                    handler: ...
                    excluded_404s:
                        - ^/phpmyadmin

    .. code-block:: xml

        <!-- config/packages/prod/monolog.xml -->
        <container xmlns="http://symfony.com/schema/dic/services"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:monolog="http://symfony.com/schema/dic/monolog"
            xsi:schemaLocation="http://symfony.com/schema/dic/services
                http://symfony.com/schema/dic/services/services-1.0.xsd
                http://symfony.com/schema/dic/monolog
                http://symfony.com/schema/dic/monolog/monolog-1.0.xsd">

            <monolog:config>
                <monolog:handler type="fingers_crossed" name="main" handler="...">
                    <!-- ... -->
                    <monolog:excluded-404>^/phpmyadmin</monolog:excluded-404>
                </monolog:handler>
            </monolog:config>
        </container>

    .. code-block:: php

        // config/packages/prod/monolog.php
        $container->loadFromExtension('monolog', array(
            'handlers' => array(
                'main' => array(
                    // ...
                    'type'          => 'fingers_crossed',
                    'handler'       => ...,
                    'excluded_404s' => array(
                        '^/phpmyadmin',
                    ),
                ),
            ),
        ));
