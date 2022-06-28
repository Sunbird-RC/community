---
description: >-
  This is a common library created to be used in any Sunbird RC platform for VC
  verification.
---

# VC Verification Module

This NPM module is developed for scanning the QR(Quick Response) codes inside your Angular applications. This library is designed to be used in sunbird RC platforms.

**To use this module you need to follow a few steps.**

1. Install vc-verification npm package in your angular project. Using below command

```
npm i vc-verification
```

2\. You need to install the @zxing/ngx-scanner npm module. Before install this npm module you need to check your angular compatible version with this package version. You can check the details below too.

Run cmd -

```
npm i @zxing/ngx-scanner@vx.x.x
```

| Angular Version                           | Ngx-scanner version            |
| ----------------------------------------- | ------------------------------ |
| Angular 9.0.0 <10                         | v3.0.0, v3.0.1                 |
| Angular 10.1.5                            | v3.1.0, v3.1.1, v3.1.2, v3.1.3 |
| Angular 11.2.11 \|\| ^12.0.4              | V3.2.0, v3.3.0                 |
| Angular 11.2.11 \|\| ^12.0.4 \|\| ^13.0.0 | V3.4.2, v3.5.0                 |

\
For more detail about compatible version you can check this doc - [https://www.npmpeer.dev/packages/@zxing/ngx-scanner/compatibility](https://www.npmpeer.dev/packages/@zxing/ngx-scanner/compatibility)

