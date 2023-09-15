---
description: >-
  Sunbird RC support configuring a custom design for the QR code instead of a
  plain black coloured QR Code
---

# Custom QR Code design

#### Below are the steps to configure custom designed QR code

* The below ENV needs to be configured for `certificate-api` service

`ENABLE_CUSTOM_QR_CODE_CANVAS: true`

* Create a `qr_code_config.json` file. And add the QR code styling which can be designed here, [https://qr-code-styling.com/](https://qr-code-styling.com/)
* A sample styling is available here, [https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/services/certificate-api/configs/qr\_code\_config.json](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/services/certificate-api/configs/qr\_code\_config.json)
* Once the styling file is added, the file needs to be mounted to `certificate-api` service as below

```
certificate-api:
    image: ghcr.io/sunbird-rc/sunbird-rc-certificate-api:latest
    volumes:
      - ./imports/qr_code_config.json:/app/configs/qr_code_config.json
```
