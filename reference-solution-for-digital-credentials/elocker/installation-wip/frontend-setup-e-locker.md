# Frontend Setup E-locker

**Prerequisite:**

***

* Angular 10.2.1 - find more information here [https://angular.io/guide/setup-local](https://angular.io/guide/setup-local)
* Make sure your node and npm versions are  - node v16.13.1 (npm v8.1.2)
* You can also manage these versions using [Node Version Manager](https://www.freecodecamp.org/news/node-version-manager-nvm-install-guide/)&#x20;

***

To set up E-locker on your local machine these steps should followed :&#x20;

* Use these commands to clone the repositories.

&#x20;  `git clone` [`https://github.com/Sunbird-RC/sunbird-rc-elocker-ui`](https://github.com/Sunbird-RC/sunbird-rc-elocker-ui)&#x20;

* Clone [demo-elocker](https://github.com/Sunbird-RC/demo-elocker) repository and navigate to ui-config folder

&#x20;  `git clone` [`https://github.com/Sunbird-RC/demo-elocker`](https://github.com/Sunbird-RC/demo-elocker)

&#x20;  `cd` [`demo-elocker`](https://github.com/Sunbird-RC/demo-elocker)`/ui-config`

* In this folder you will see a list of JSON files. We need to create a symbolic link of these files in the assets folder of [sunbird-rc-elocker-ui](https://github.com/Sunbird-RC/sunbird-rc-elocker-ui) repository
* For this : copy the path of ui-config folder and navigate to assets/config folder of [sunbird-rc-elocker-ui](https://github.com/Sunbird-RC/sunbird-rc-elocker-ui)
* Create a symbolic link&#x20;

&#x20;`cd sunbird-rc-elocker-ui/src/assets/config/`

&#x20;`ln -s <path_of_ui_config_from_demo_elocker>`

_For example:_ &#x20;

_> ln -s /home/user/_[_demo-elocker_](https://github.com/Sunbird-RC/demo-elocker)_/ui-config_

\--------------

* Navigate to the src folder of [sunbird-rc-elocker-ui](https://github.com/Sunbird-RC/sunbird-rc-elocker-ui)
* Install dependencies and run the application using these commands

&#x20;

&#x20; `cd sunbird-rc-elocker-ui/src/`

&#x20; `Nvm use v16.13.1`

&#x20; `yarn`

&#x20;  `npm start`\
\


* &#x20;Once the application has started successfully, you will see a message indicating that the application is running. It will also provide the URL where the application is hosted locally, typically \`[http://localhost:4200](http://localhost:4200)\`.\

* Copy the URL (\`http://localhost:4200\`) and paste it into your browser to access the Angular application.

### Proxy configuration ([How to fix CORS issues](https://github.com/Sunbird-RC/sunbird-rc-ui#faqs))

* Refer this [link](https://github.com/Sunbird-RC/sunbird-rc-ui#faqs) for proxy configuration

\
