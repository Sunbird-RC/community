# Technical Components (WIP)



#### **Credential Issuance Service**

Microservice to create the credentials in the credential store. This service is part of Sunbird RC

[https://github.com/Sunbird-RC/sunbird-rc-core/pull/217](https://github.com/Sunbird-RC/sunbird-rc-core/pull/217)&#x20;

#### **Credential Schema Service**

Microservice to create the credential schema, template and render the credentials in the selected template. &#x20;

[https://github.com/Sunbird-RC/sunbird-rc-core/pull/219](https://github.com/Sunbird-RC/sunbird-rc-core/pull/219)

#### **Identity Service (DID)**

Microservice to create the unique identity for the learner and issuer for the credential.

[https://github.com/Sunbird-RC/sunbird-rc-core/pull/218](https://github.com/Sunbird-RC/sunbird-rc-core/pull/218)

#### **Middleware Service**

The Backend for frontend (BFF) microservice is written for adding authorization for the above services and ULP specific services like Aadhar Verification (pre-prod) and UDISE APIs

{% embed url="https://github.com/Sunbird-RC/ulp-bff" %}

### **Frontend Portal links**

#### Issuer/registration portal&#x20;

This is the portal to showcase the principal, teacher, and school onboarding. It has the functionality to create bulk credentials, claim approval, view issued credentials and a dashboard. You can add setup this on your server by adding config changes.

{% embed url="https://github.com/Sunbird-RC/ulp-registration-portal" %}

#### E-wallet

This web app will be for learners to view, download or share their credentials.

{% embed url="https://github.com/Sunbird-RC/ulp-ewallet" %}

#### Verification Portal

This web app will be used for verifiers to verify whether the particular credential is valid and not expired.

{% embed url="https://github.com/Sunbird-RC/ulp-vc-verification" %}
