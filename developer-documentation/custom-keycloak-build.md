# Custom Keycloak Build

SunbirdRC uses a custom keycloak image which is configured to enable/disable NONCE validation. The required changes are made in this repository, [https://github.com/Sunbird-RC/keycloak/tree/configurable-nonce-validation](https://github.com/Sunbird-RC/keycloak/tree/configurable-nonce-validation).

NONCE validation is default enabled in keycloak, to turn off the validation `VALIDATE_NONCE` should be set to `"false".`

This configuration is not provided by keycloak by default even in the latest version. We have configured this change, particularly for enabling Digilocker Meripehchaan SSO (Task: [https://github.com/Sunbird-RC/community/issues/593](https://github.com/Sunbird-RC/community/issues/593)).

### Build custom keycloak image

#### Pre-requisites

* JAVA 11 (tested with 11.0.8)
* Maven

#### Build keycloak distribution jar

* Clone [https://github.com/Sunbird-RC/keycloak/tree/configurable-nonce-validation](https://github.com/Sunbird-RC/keycloak/tree/configurable-nonce-validation) the repository (Contains the source code)
* Run the below command to generate the distribution jar. Reference [https://github.com/Sunbird-RC/keycloak/blob/configurable-nonce-validation/docs/building.md](https://github.com/Sunbird-RC/keycloak/blob/configurable-nonce-validation/docs/building.md) `mvn clean install -Pdistribution`&#x20;
* The above command should create `keycloak-14.0.0.tar.gz` in `distribution/server-dist/target` directory&#x20;

#### Build keycloak docker image

* Clone [https://github.com/keycloak/keycloak-containers/tree/main/](https://github.com/keycloak/keycloak-containers/tree/main/) the repository (Contains the build files)
* `cd server`
* Run a Python HTTP server in the [keycloak repo](custom-keycloak-build.md#build-keycloak-distribution-jar) to access the distributed jar file. \
  `python -m http.server 8001`
* Build the keycloak docker image,\
  `docker build -t sunbirdrc/keycloak --build-arg KEYCLOAK_DIST=http://<YOUR_IP_ADDRESS>:8001/keycloak-14.0.0.tar.gz .`
* Tag the new docker image and publish it to dockerhub / docker registry



