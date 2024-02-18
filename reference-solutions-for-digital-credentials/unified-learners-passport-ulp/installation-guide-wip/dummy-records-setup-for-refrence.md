# Dummy records setup for refrence

If you want to create dummy records in your local/server to get the running demo.&#x20;

Clone the below script for dummy records - (move this to Sunbird RC repo)

{% embed url="https://github.com/Unified-Learner-Passbook/setup-script" %}

Once you clone the repository, add a .env file with the following variables

`ULPCLI_VERSION=v1.0`

`ULPCLI_NAME=ULP NEST CLI`

`BULK_ISSUANCE_URL=http://localhost:3007`

`DEFAULT_PASSWORD=abcd@123`

`CLIENT_USERNAME=ulp-admin`

`EWALLET_URL=http://localhost:3000`

`VERIFICATION_URL=http://localhost:3002`

Run&#x20;

```
npm install
npm run cli ulp
```

