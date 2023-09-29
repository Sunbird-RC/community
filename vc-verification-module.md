---
description: >-
  This is a common library created to be used in any Sunbird RC platform for VC
  verification.
---

# VC Verification Module

This NPM module is developed for scanning the QR(Quick Response) codes inside your Angular applications. This library is designed to be used in sunbird RC platforms.

### **To use this module you need to follow the below steps**

1. Install the vc-verification npm package in your angular project. Using the below command

```
npm i vc-verification
```

**2.** The <mark style="color:red;">deprecated</mark> `ngx-scanner` has been removed starting from version 0.0.13

&#x20;You need to install the @zxing/ngx-scanner npm module. Before installing this npm module you need to check your angular compatible version with this package version. You can check the details below too.

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

3\. Add below CND link in index.html file

```
  <script src="https://cdn.jsdelivr.net/npm/@undecaf/zbar-wasm@0.9.12/dist/index.js"></script>
```

4\. Import **vc-verification** and **@zxing/ngx-scanner** library in your **app.module.ts** file

```
import { VerifyModule } from 'vc-verification';
import { ZXingScannerModule } from '@zxing/ngx-scanner'; <- not required >=0.0.13

import * as configData from '../assets/config.json';  // Read config from .json file

Or
// Add configuration in app.module.ts file
const configData = {
  baseUrl: "assets/api/event-detail.json",
 }

@NgModule({
  declarations: [
   AppComponent
  ],
  imports: [
  .....
  ......
    VerifyModule.forChild(configData),  < —
    ZXingScannerModule, < — not required >=0.0.13
    .....
  ......
  ],
  providers: [],
  bootstrap: [AppComponent]
})

export class AppModule { }

```

4.1 Anyone can use the Default Scan template using the sb-vc-verify selector or he/she can use only verify service too.

Add below code in your app.component.html file

```
 <sb-vc-verification> </sb-vc-verification>
```

4.2 If anyone wants to change labels and messages from template. You can pass it through item property.

app.component.html

```
<sb-vc-verification [item]="itemData"></sb-vc-verification>
```

App.component.ts : Change the value of key

```
this.itemData =
 {
      "scanner_type": "ZBAR_QRCODE",
      "showResult": [
        { "title": "Name", "path": "credentialSubject.name" },
        { "title": "Father Name", "path": "credentialSubject.fatherName" },
        { "title": "Date of Issuance", "path": "issuanceDate",  'type' : 'date' },
        { "title": "ABHA Number", "path": "credentialSubject.id",  "removeStr" : "did:abha:" },
        { "title": "NOTTO ID", "path": "credentialSubject.nottoId" },
        { "title": "Organs", "path": "credentialSubject.pledge.organs" },
        { "title": "Tissues", "path": "credentialSubject.pledge.tissues" },
        {  "title": "Emergency Contact Details", "path": "credentialSubject.emergency.mobileNumber" }
      ],
      "scanNote": "To verify pledge certificate, simply scan the QR code that's on the document.",
      "certificateTitle": 'Pledge Certificate',
      "verify_another_Certificate": 'Verify another Certificate',
      "cetificate_not_valid": 'This Certificate is not valid',
      "scan_qrcode_again": "Please scan QR code again"
    });
```

This library supports the two QR code scanners (ZXING\_QRCODE and ZBAR\_QRCODE). By default enable the ZXING\_QRCODE scanner to scan QR code. If you want to change the scanner, the user needs to set the **scanner\_type** property value to the **ZBAR\_QRCODE**.

Difference between ZXING\_QRCODE and ZBAR\_QRCODE.

* ZXING\_QRCODE doesn't scan QR code which contains a large amount of data, it's working fine with simple QR codes (<mark style="color:red;">Deprecated</mark>).
* ZBAR\_QRCODE supports fast scanning and easily scans QR code which contains large amounts of data.&#x20;

_Starting from version 0.0.13 and onwards, the default scanner type has been changed to **ZBAR\_QRCODE** as ZXING\_QRCODE scanner has been removed by default._

From v10 introduced one more **showResult** object under itemData to configure what certificate data the user wants to display on the result card after verifying QR code. In this object user need to add property path and title which is display on verified card.

* to show date on card, you wants to add **type** property with **date** value
* if you want remove any string from value, you can achieve this by using **removeStr** property

4.3 If anyone wants to use their own he/she can implement their own UI and he/she can use service methods of vc-verification library.

**App.component.ts**: Import service file in your component where you want to call vc-verification library service method call.

```
import { VerifyService } from 'verify-module'; 

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
 
  constructor(public verifyService: VerifyService) {

    this.verifyService.scanSuccessHandler(event).then((res) => {
      console.log(res);
    })
  }

}
```

**List of service methods**

| Method Name                     | Parameter | Description                          |
| ------------------------------- | --------- | ------------------------------------ |
| enableScanner()                 | -         | To Hide/show scanner screen          |
| scanSuccessHandler($event: any) | $event    | This method used to verify scan data |

## Change Button Color

To change the button color, user need to override **vc-btnPrimary** css class in stye.css

## Source Code

The source code to install this example can be found at [https://github.com/Sunbird-RC/vc-verification](https://github.com/Sunbird-RC/vc-verification)
