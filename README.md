# sulu-sylius-integration

Bootstrap repository which contains glue to run Sulu-Sylius example application.

## Installation

### 1. Initialize repositories

```
git clone git@github.com:wachterjohannes/sulu-sylius-integration.git
cd sulu-sylius-integration
git submodule update
```

### 2. Start docker container

```
docker-compose up
```

### 3. Initialize Sulu Application

```
cd sulu-sylius
composer update
```

You can leave the parameters as they are.

```
bin/console sulu:build dev --destroy
bin/console server:run 127.0.0.1:8000
```

To consume messages from rabbit-mq (produced by sylius plugin) run following
command in a seperate terminal:

```
bin/console rabbitmq:consumer sulu_sylius
```

### 4. Initialize Sylius Application

```
cd SuluSyliusIntegrationPlugin
composer update
cd tests/Application
yarn install
yarn run gulp
bin/console assets:install web -e test
bin/console doctrine:database:create -e test
bin/console doctrine:schema:create -e test
bin/console sylius:fixtures:load -e test
bin/console server:run -d web 127.0.0.1:8001 -e test
```

### 5. Usage

To see if everything works together you can go to `http://127.0.0.1:8000/admin/#articles/en` (user: "admin" and
password "admin") and see if there are already synced products.

TODO screenshot of article list

You should see a bunch of them (already created by the sylius fixtures). If you publish a few products you should be
able to see them on the Website Homepage `http://127.0.0.1:8000/`. There you can goto a product and add it to the cart.

TODO screenshot of cart

After you finished filling up your cart you can follow the link `Add it to cart` and finish the checkout in the sylius
application.

TODO screenshot of checkout

To review your cart goto `http://127.0.0.1:8001/admin` and use the Sylius admin.

## Disclaimer

This is only a running prototype for a integration of Sylius/Sulu application's and is far away from production ready.
You can use this as a base for your project but use it wisely.
