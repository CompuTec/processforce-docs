# CompuTec License Server Installation

This document provides a step-by-step guide on how to install and configure CompuTec License Server.

:::info

You can check the available **ProcessForce user license types** [**here**](./../license-chart.md).

:::

---

## Installing CompuTec License Server

:::danger

Remove an old version of ProcessForce License Server and SAP COM License Bridge in case you used them previously (with ProcessForce 8.81 / 8.82 / 9.0 PL05 - PL08 HotFix) before installing a new version.

::::

:::caution

CompuTec License Server can be installed in a Windows environment only.

:::

1. Download CompuTec License Server from **ADD LINK** here.

2. Extract the ZIP file.

3. Run the CompuTec.LicenseServer.Setup.msi file.

4. Click the Next button and go through the installation process.

## Setting Windows Firewall rule

1. Open: Windows Settings → Network & Internet → Windows Firewall:

  ![Firewall rule](./../media/firewall-rule.png)
2. Click the Advanced settings option:

  ![Firewall rule - advance settings](./../media/firewall-rule-2.png)
3. In Windows Firewall with Advanced Security window, select Inbound Rules and click New Rule... in the Actions section – this will run New Inbound Rule Wizard:

  ![New rule](./../media/new-rule.png)
4. Choose Port as a rule type:

  ![Rule type](./../media/rule-type.png)
5. Choose TCP protocol and specify 30002 local port:

  ![Local port](./../media/rule-type.png)
6. Choose the Allow the connection action:

  ![Action](./../media/action.png)
7. Choose all profiles:

  ![Profile](./../media/profiles.png)
8. Specify the rule name and click Finish:

  ![Rule name](./../media/name.png)

## Using CompuTec Service Manager and configuring Direct Data Access mode

You can find the guide [here](./direct-access.md).
