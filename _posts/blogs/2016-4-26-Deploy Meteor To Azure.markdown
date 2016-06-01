---
layout: blog
category: blogs
title: 'Deploy Meteor Application to Microsoft Azure'
date: '2016-4-26'
author: Kgao
tags: 'Meteor.js, Windows Azure, Virtual Machine'
public: true
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


So far as you know Meteor official support team just ends their Galaxy free hosting in the end of March. Which is sad... :(
However, that's life! When you seeking some other replacement for your hosting, you might find some articles recently talking about deployment on AWS, Digital Ocean, Heroku...etc.
For windows users, and users who are planning to or currently using Microsoft Azure, there might be lack of articles talking about it before Azure add native support for Meteor. As an explorer of web technoloy, I believe everything is possible, so I spent some time to figure out new deployment solutions for Meteor apps in Microsoft Azure.


Let's assume you already have:

1. A Meteor app running locally.  (Check how to build your Meteor app [here](http://www.meteor.com))
2. Azure account. (Get your free Azure account [here](https://azure.microsoft.com/en-us/free/))

## ***Background***

### Understand Azure
[Azure](https://azure.microsoft.com/en-us/overview/what-is-azure/) offers several ways to host web apps:

- [App Service](https://azure.microsoft.com/en-us/services/app-service/) (Very beginner-friendly, less manual) 

- [Cloud Services](https://azure.microsoft.com/en-us/services/cloud-services/) (Beginner-friendly, and flexible) 

- [Virtual Machines](https://azure.microsoft.com/en-us/services/virtual-machines/) (Less beginner-friendly, but much more flexible). 

- [Service Fabri](https://azure.microsoft.com/en-us/services/service-fabric/) (Less beginner-friendly, microservice-based application development)
    
    ![screenshot](../../../../../img/screenshots/azure-cloud-3-services.png)

    For more detailed comparison, see [here](https://azure.microsoft.com/en-us/documentation/articles/choose-web-site-cloud-service-vm/).

## ***Deployment***

### Deploy to Azure App Service

1. Build App Srvice using Azure Portal Or try it [here](https://tryappservice.azure.com/).
   (Note: Please don't forget to set your `deployment credentials`.)

2. Configure App Service

    Add the following enironment variables in your Application Settings
   
        - WEBSITE_NODE_DEFAULT_VERSION : `0.10.40`
        - ROOT_URL : `http://{sitename}.azurewebsites.net` or your custom domain if you've set that up
        - MONGO_URL : (Mongo DB connection string from a MongoDB hosted on [MongoLab](https://mlab.com/) or a VM)
        - METEOR_SETTINGS : JSON value equals to the entire contents of your `settings.json` file
    
    
3. Demeteorize and deploy

    So far, Azure hasn't enable native support for Meteor, you can track insider [here](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6848937-add-support-for-meteor-on-azure-websites).
    
    However, you can demeteorize your meteor app to run as a "standard" nodejs app to take advantage of Azure App Service's native support with features like `high availability with auto-patching`, `built-in autoscale` and `load balancing`. 
    
     
    For **Windows** users, you can use [Azure-demeteorizer](https://github.com/christopheranderson/azure-demeteorizer).
    Make sure you install and meet all the [prerequisites](https://github.com/christopheranderson/azure-demeteorizer#prerequisites) first, (Note: Visual Studio 2013 would be the most comaptible choice for most users.)
    then Azure-demeteorizer can do all the magic for you (Thanks to my colleague Christopher, this is a great tool for windows users!):
    
```javascript
> 1. npm install -g christopheranderson/azure-demeteorizer
> 2. cd [YourMeteorApp]
> 3. azure-demeteorizer build
> 4. azure-demeteorizer install
> 5. azure-demeteorizer zip
> 6. azure-demeteorizer deploy -s [sitename] -u [username] - p [password]`
    //- sitename: the name of your App.
    //- username: username for your site's [deployment credentials].
    //- password: password for your site's [deployment credentials].
```

For **Linux/Mac OSX** users, you can use [Demeteorizer](https://github.com/OnModulus/demeteorizer), however,
you have to demeteorize first, and do a continuous deployment with Git, TFS, GitHub, or Visual Studio Team Services (Here use Git as example):

```javascript    
    > 1. $ npm install -g demeteorizer
    > 2. $ cd [YourMeteorApp]
    > 3. $ demeteorizer
    > 4. $ cd .demeteorized/bundle/programs/server
    > 5. $ npm install
    > 6. $ MONGO_URL=[YourMongoDBURL] PORT=[Number] ROOT_URL=[YourAppRootUrl] npm start
    //If this is successful, then you can continue using `git remote` to push your app to Azure. 
    //If you didn't choose continuous deployment using Git when you build your Azure App Service,
    //you can manually set it up:
    //https://azure.microsoft.com/en-us/documentation/articles/web-sites-publish-source-control/
    > 7. $ git init
    > 8. $ git remote add azure "https://username@{sitename}.scm.azurewebsites.net:443/{appname}.git"
    > 9. $ git add .
    > 10. $ git commit -am "Initial Commit"
    > 11. $ git push azure master
```


### **Deploy to Azure Virtual Machine**

1. Build VM by Azure Portal Or try it [here](https://azure.microsoft.com/en-us/trial/free-trial-virtual-machines/).
   (Note: Please don't forget to set your deployment credentials.)
2. Mup and Deploy

    > If you build Windows VMs, (i.e. Windows Server 2012) check previous chapter about how to demetoerize and deploy your app to Azure.
    
    > If you build Linux VMs, (i.e. Ubuntu 14.04 LTS) you can use [Mupx](https://github.com/arunoda/meteor-up/tree/mupx), please continue the following steps:

```javascript     
> 1. Install mupx globally using npm: `npm install -g mupx`
> 2. `cd tutorial`
> 3. `mupx init` to generate or manually add:   
         * mup.json // Meteor Up configuration file, 
         //see exmpale: https://github.com/arunoda/meteor-up/tree/mupx#example-file
         * settings.json // Settings for Meteor, 
         //settings API: http://docs.meteor.com/#meteor_settings
> 4. mupx setup
> 5. mupx deploy
```

### Enable SSL
- If you have deployed your app to Azure App Service:

    - By default, Azure already enables HTTPS for your app with a wildcard certificate for the *.azurewebsites.net domain. 
    If you don't plan to configure a custom domain, then you can benefit from the default HTTPS certificate. 
    Read more [here](https://azure.microsoft.com/en-us/documentation/articles/web-sites-configure-ssl-certificate/)

    - For a better performance and connectivity, you can turn on websockets instead of long-polling in your App Service, simply by one-click in `Application settings` through Azure portal.

- If you have deployed your app to VMs using Mupx:
    
    - You can add SSL support following [here](https://github.com/arunoda/meteor-up/tree/mupx#ssl-support), however, you have to obtain your own SSL certificate from a certificate authority (CA) and a private key first. 


- For other deployments, you might set up your SSL manually.


## ***Conclusion***

### **Deployment Choices**
    
```html
| Client OS | Server OS             | Solutions(Recommended) | Status | 
|-----------|-----------------------|------------------------|--------|
| Windows   | Windows               | Azure-demeteorizer     |    ✔   |
| Windows   | Linux                 | Mupx                   |    ✔   |
| Linux     | Windows               | Demeteorizer           |    ✔   |
| Linux     | Linux                 | Mupx                   |    ✔   |
```
*For other deployments on Modulus, DigitalOcean, Galaxy, please check the official deployment book [here](http://meteortips.com/deployment-tutorial/).



Thank you for reading, hope this will benefit your daily work. 
Feel free to leave your comment here and share with others!
:)