---
description: >-
  leverage existing trustworthy sources of data to accelerate registry
  implementation
---

# Leveraging Existing data stores

### Why

If you are implementing a registry in a scenario where some pre-work has already happened & data exists in the form of some sort of a database, it might be prudent \(if the data is deemed accurate & trustworthy\) that some sort of an import mechanism be facilitated.  

### How

If an existing database or directory where data exists which needs to be leveraged, following are the scenarios in which it can be done without compromising the core registry principles.

**Invitation for Registration Scenario**

Existing databases can be used to trigger invitations for registration to the respective registries. 

**Leverage existing Data for Registration, Authentication & data pre-fill**

1. User logs in using existing SSO system 
2. Select Data from connected database is pre filled in registration form based on primary key \(email/phone/GST/ UDISE ID\) 
3. User ensures the pre filled information is correct & registers
4. Data Fields \(apart from those that are self claimed\) that need verification will need to be verified via prevalent systems - eg : Phone number & email via OTP or Date of Birth via Aadhar. 

**Leverage existing data for pre-filling Registration form**

1. User registers themselves on the registry
2. On the page where they need to fill detailed information a button is provided to allow the user to import data fields from an existing system
3. Select Data from connected database is pre filled in registration form based on primary key \(email/phone/GST/ UDISE ID\) 
4. User ensures the pre filled information is correct & registers
5. Data Fields \(apart from those that are self claimed\) that need verification will need to be verified via prevalent systems - eg : Phone number & email via OTP or Date of Birth via Aadhar. 

