# A Decoupled Approach for the Electronic Invoicing
[![](https://i.imgur.com/fn8PnPn.jpg)]()

## Overview
This application creates electronic invoices based on SAP Business ByDesign [Custormer Invoice Documents](https://help.sap.com/viewer/2754875d2d2a403f95e58a41a9c7d6de/LATEST/en-US/2ce68f24722d1014b38dcc6246f15f03.html). When invoked, it pulls the data from ByD OData services, based on the message data, and calls the Tax Authority web service (in this case [AFIP](https://www.afip.gob.ar/ws/)) to issue the electronic invoice. Once it is authorized and created, the solution writes back the signature data into the ByD Customer Invoice extension field. We adopted the [SAP Cloud Platform Enterprise Messaging](https://blogs.sap.com/2020/10/21/scp-enterprise-messaging-for-the-smbs) as the default message broker service and [SAP Business ByDesign](https://www.sap.com/products/business-bydesign.html) as the core ERP.

## Pre requisites
* SAP Cloud Platform Enterprise Messaging
* Credentials to access [AFIP web services](https://www.afip.gob.ar/ws/)
* SAP Business ByDesign Tenant
* A [ByD Custom Odata Service](https://github.com/SAP-samples/sapbydesign-api-samples/) for [Customer Invoice](https://github.com/SAP-samples/sapbydesign-api-samples/blob/master/Custom%20OData%20Services/khcustomerinvoice.xml)

**NOTE 1:** you must create a new extension field on customer invoice document, and add it to the OData service exposed above
**NOTE 2:** Although we provide AFIP to issue the Electronic Invoice in this prototype, you can adapt the code to consume the Tax Authority web services you need.

## Deployment
Clone or download this repository:
```bash
git clone https://github.com/B1SA/eInvoiceAfip.git
```
From its root folder, open the terminal, login to your SCP account and push it.
```bash
cf push --random-route
```
After it finishes the deployment, set up the environment variable for:
* ERP_URL: your ERP URL, as a fallback in case there's no indication in the consumed message;
* BYD_B64AUTH: ByD User name and password, encoded in base64, to access the OData web services;
* AFIP_TOKEN: your token provided by AFIP;
* AFIP_SIGN: your sign provided by AFIP;
* AFIP_CUIT: your company's Unique Tax Identification Code.

```bash
cf set-env <app_name> ERP_URL <your_ERP_URL>
cf set-env <app_name> BYD_B64AUTH <your_ERP_URL>
cf set-env <app_name> AFIP_TOKEN <your_AFIP_TOKEN>
cf set-env <app_name> AFIP_SIGN <your_AFIP_SIGN>
cf set-env <app_name> AFIP_CUIT <your_AFIP_CUIT>
```
For details on how to deploy apps on SCP CF check this tutorial: [Get Started with SAP Cloud Platform Trial](https://developers.sap.com/tutorials/cp-trial-quick-onboarding.html)

### Configuring SAP Cloud Platform Enterprise Messaging
You must create at least one queue and subscribe a webhook to the queue, pointing to the application you have just deployed above. For details on how to create queues and subscribe webhooks, check this blog post: [SCP Enterprise Messaging for the SMBs](https://blogs.sap.com/2020/10/21/scp-enterprise-messaging-for-the-smbs)

## License
This proof of concept is is released under the terms of the MIT license. See [LICENSE](LICENSE) for more information or see https://opensource.org/licenses/MIT.
 
## Support and Contributions
This repository is provided "as-is". No support is available. Feel free to open issues or provide pull requests.

