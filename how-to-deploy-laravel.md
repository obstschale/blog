# How to deploy Laravel application 

<p style="display:flex;justify-content:center;">
    <img src="/images/logos/laravel.png" alt="Laravel" style="height:200px;">
</p>


Apparently you already have some **Laravel application** and some **server** or **shared hosting**. 
Now you need to automate the process of **deployment**. 
Deployer will helps you in this as it ships with some ready to use recipes for **Laravel** based application. 

Let's start with [installation](/docs/installation) of Deployer. Run next commands in terminal: 

```sh
curl -LO https://deployer.org/deployer.phar
mv deployer.phar /usr/local/bin/dep
chmod +x /usr/local/bin/dep
```

Next, in your projects directory run:

```sh
dep init -t Laravel
```

Command will create `deploy.php` file for *deploying Laravel*. This file called *recipe* and based on built-in recipe *laravel.php*.
It's contains some server configuration and example task. 

First, we need to configure `repository` config of our application:

```php
set('repository', 'git@github.com:user/project.git');
```

Second, configure server:
 
```php
server('production', 'domain.org')
    ->user('name')
    ->identityFile()
    ->set('deploy_path', '/var/www/html');
```

Best way to authenticate using key. By default `identityFile` will look for private and public keys in `~/.ssh/id_rsa` 
and `~/.ssh/id_rsa.pub` accordingly. If you want to use different keys, or your key have password:

```php
identityFile('~/.ssh/id_rsa.pub', '~/.ssh/id_rsa', 'password')
```

Another important parameter it is `deploy_path`, where you project will be located on server. 

Let's do our first deploy:

```sh
dep deploy
```

If every think goes well, deployer will create next structure on server in `deploy_path`:

```text
├── .dep
├── current -> releases/1
├── releases
│   └── 1
└── shared
    ├── .env
    └── storage
```

* `releases` dir contains *deploy* releases of *Laravel* application,
* `shared` dir contains `.env` config and `storage` which will be symlinked to each release,
* `currect` is symlink to last release,
* `.dep` dir contains special metadata for deployer (releases log, `deploy.log` file, etc).

Configure you server to serve files from `current`. For example if you are using nginx next:

```config
server {
  listen 80;
  server_name domain.org;

  root /var/www/html/current/public;

  location / {
    try_files $uri /index.php$is_args$args;
  }
}
```

Now you will be able to serve you **laravel project**:

<div style="text-align: center; margin: 50px 0;">
    <img src="/images/screenshot/laravel.png" alt="Deploy Laravel Application" style="width: 60%; box-shadow: 0px 10px 30px rgba(128, 128, 128, 0.52);">
</div>

If you want to automatically migrate database, *Laravel* recipe ships with `artisan:migrate` task. Add this lines to your `deploy.php`:

```php
after('deploy:update_code', 'artisan:migrate');
```

More about configuration and task declarations in our [documentation](/docs).

...
