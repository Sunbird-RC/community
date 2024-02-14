# Installation(WIP)

## Backend Setup

### Source Code

The source code to install this example can be found at&#x20;

{% embed url="https://github.com/Sunbird-RC/demo-certificate-issuance" %}

### Project link

{% embed url="https://github.com/orgs/Sunbird-RC/projects/6" %}

## Frontend Setup&#x20;

### Prerequisite:

* Angular 10.2.1 - find more information here [https://angular.io/guide/setup-local](https://angular.io/guide/setup-local)
* Make sure your node and npm versions are  - node v16.13.1 (npm v8.1.2).You can also manage these versions using [Node Version Manager](https://www.freecodecamp.org/news/node-version-manager-nvm-install-guide/)&#x20;

For Issuance portal two repos need to be setup&#x20;

1. [Sunbird-rc-issuance-ui](https://github.com/Sunbird-RC/sunbird-rc-issuance-ui) - which contains the main project
2. [Demo-certificate-issuance](https://github.com/Sunbird-RC/demo-certificate-issuance) - which contains reference ui config files

To set up Issuance on your local machine these steps should followed :&#x20;

1. Navigate to these repositories and create a fork

{% embed url="https://github.com/Sunbird-RC/sunbird-rc-issuance-ui" %}

{% embed url="https://github.com/Sunbird-RC/demo-certificate-issuance" %}

2. Use these commands to clone the repositories

```
git clone https://github.com/Sunbird-RC/sunbird-rc-issuance-ui 
```

3. Clone [demo-certificate-issuance](https://github.com/Sunbird-RC/demo-certificate-issuance) repository and navigate to ui-config folder

```
git clone https://github.com/Sunbird-RC/demo-certificate-issuance
cd demo-certificate-issuance/ui-config
```

* In this folder you will see a list of JSON files. We need to create a symbolic link of these files in the assets folder of sunbird-rc-issuance-ui repository.
* For this: copy the path of ui-config folder and navigate to assets/config folder of sunbird-rc-issuance-ui
* Create a symbolic link&#x20;

```
cd sunbird-rc-issuance-ui/src/assets/config/
ln -s <path_of_ui_config_from_edu_core_registries>

```

For example:  ln -s /home/user/Issuance/demo-certificate-issuance/ui-config

\
**demo-certificate-issuance**

* Navigate to the src folder of sunbird-rc-issuance-ui
* Install dependencies and run the application using these commands

```
cd sunbird-rc-issuance-ui/src/
Nvm use v16.13.1
yarn
npm start
```

### Proxy configuration (How to fix CORS issues)

To avoid CORS issues you can use proxy configuration.

**Step 1** Firstly check a <mark style="color:red;">proxy.conf.json</mark> file in the root folder.

**Step 2** Now we have to create the proxy configuration for API endpoints. So add your proxy configuration in your <mark style="color:red;">proxy.conf.json</mark> file as given in the below format-

```
"/registry/api/docs": {
"target": "https://domainname/",
"secure": false,
"changeOrigin": true,
"logLevel": "debug"
}
```

#### Definition of parameters is given below-

"/echo” is your API Path.\
"target" is your domain name where your API’s are hosted.\
"secure" is a boolean type parameter, if your domain has SSL then you should use true else you should use false as value.\
"changeOrigin" should be true if your backend is not hosted on localhost server.\
“logLevel" is used to check whether a proxy is working or not. Proxy log levels are info (the default), debug, warn, error, and silent.

**Step 3:** Now finally we have to replace the domain name with <mark style="color:blue;">http://localhost:4200/</mark> from the <mark style="color:red;">config.json</mark> file. And Run npm start or ng serve <mark style="color:red;">--proxy-config proxy.conf.json.</mark>

