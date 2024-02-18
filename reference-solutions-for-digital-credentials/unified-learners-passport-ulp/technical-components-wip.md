# Technical Components (WIP)

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/ULP - High level Diagram.jpg" alt=""><figcaption><p>High level diagram of ULP depicting the interplay between the ecosystem players</p></figcaption></figure>

</div>

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/ULP - Packaging.png" alt=""><figcaption><p>Detailed view of ULP components</p></figcaption></figure>

</div>

Technical&#x20;

The components can be divided into two parts - **Microservices / APIs and Frontend Portals**

### **A. ULP Microservices**

#### **1. Credential Issuance Service**

Microservice to create the credentials in the credential store. This service is part of Sunbird RC

[https://github.com/Sunbird-RC/sunbird-rc-core/pull/217](https://github.com/Sunbird-RC/sunbird-rc-core/pull/217)&#x20;

#### **2. Credential Schema Service**

Microservice to create the credential schema, template and render the credentials in the selected template. &#x20;

[https://github.com/Sunbird-RC/sunbird-rc-core/pull/219](https://github.com/Sunbird-RC/sunbird-rc-core/pull/219)

#### **3. Identity Service (DID)**

Microservice to create the unique identity for the learner and issuer for the credential.

[https://github.com/Sunbird-RC/sunbird-rc-core/pull/218](https://github.com/Sunbird-RC/sunbird-rc-core/pull/218)

#### **4. Middleware Service**

The Backend for frontend (BFF) microservice is written for adding authorization for the above services and ULP specific services like Aadhar Verification (pre-prod) and UDISE APIs

{% embed url="https://github.com/Sunbird-RC/ulp-bff" %}

### **B. Frontend Portal links**

#### 1. Issuance portal&#x20;

This is the portal to enable the organisation, issuer onboarding. It has the functionality to create bulk credentials, claim approval, view issued credentials and a dashboard. You can add setup this on your server by adding config changes.&#x20;

_The portal is currently under maintenance and might not be usable._

{% embed url="https://github.com/Sunbird-RC/ulp-registration-portal" %}

#### 2. Holder App

This web app will be for learners to view, download or share their credentials.

{% embed url="https://github.com/Sunbird-RC/ulp-ewallet" %}

#### 3. Verification Portal

This web app will be used for verifiers to verify whether the particular credential is valid and not expired.

{% embed url="https://github.com/Sunbird-RC/ulp-vc-verification" %}
