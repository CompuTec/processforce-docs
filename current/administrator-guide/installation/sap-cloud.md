---
sidebar_position: 4
---

# SAP Business One Cloud environment

Here you can find notes on registering ProcessForce in the SAP Business One Cloud environment.

---

## Registering ProcessForce

For a guide on registering add-ons, deploying to Service Unit, and assigning to Tenants in Cloud Control Center, see [the SAP Business One Cloud Administrator's Guide](https://help.sap.com/docs/SAP_BUSINESS_ONE_CLOUD), Managing Extensions chapter.

## Error while deploying ProcessForce to Service Unit in Cloud Control Center

With an older version of SAP Business One Cloud / Cloud Control Center, you may run into a problem deploying the ProcessForce add-on to Service Unit – the issue is related to an error while registering the Microsoft COM DLL file (RegisterActiveX.dll). In that case, please create a ticket on our support portal requesting a version of the ProcessForce installer that allows the add-on to be deployed appropriately.

## Closing SAP Business One client while performing activities in ProcessForce when logged in as Partner Support User (``_PSU_1`` or ``_PSU_2``)

There is a known defect causing the closure of the SAP Business One client without any message when performing activities in ProcessForce in the SAP Business One Cloud environment. It may cover several aspects, including:

- Trying to assign a user a ProcessForce license – please visit [the Licensing Issues page for more information](./../../troubleshooting/licensing-issues.md).

- Opening a ProcessForce window after at least 15 minutes of inactivity or after re-logging into the company database – please visit [the General Functions page](./../../troubleshooting/general-functions.md) for more information.
