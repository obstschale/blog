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

This command will create `deploy.php` file for *deploying Laravel*.
