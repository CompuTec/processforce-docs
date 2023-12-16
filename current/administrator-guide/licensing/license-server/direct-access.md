# Using CompuTec Service Manager and configuring Direct Data Access mode

This document provides a step-by-step guide on how to use and configure CompuTec Service Manager.

:::info

CompuTec Service Manager is a part of CompuTec License Server, which installation guide you can find on [the CompuTec License Server Installation page](./license-server-installation.md).

:::

## Starting CompuTec Service Manager

After the CompuTec License Server installation in the system, the CompuTec Service Manager shortcut is available on the Windows program list.

After clicking it, the CT icon appears in the Windows notification area:

![CT shortcut](./../media/CT-shortcut.png)

After opening the Manager, the COMPUTEC SERVICE MANAGER window appears. Here you can start/stop/restart a chosen CompuTec service:

![CT Service Manager](./../media/CT-Service-Manager_1.png)

Configuring the settings of a selected service is possible after clicking the Settings button.

Terminal Licensing is a section for managing the license of CompuTec PDC / CompuTec WMS products.

## CompuTec License Server settings

You can change Server Name and Port Number in the Server Settings tab. You do not have to change the default values:

![CT Server Settings](./../media/CT-License-Server-Settings_1_Server-Settings1.png)

You can also specify a logging level and see a disk location where logs are kept.

## Configuring CompuTec License Server database connection – enabling Direct Data Access mode

1. In the COMPUTEC LICENSE SERVER SETTINGS window, switch to the Database Connection tab:

  ![Database settings](./../media/CT-License-Server-Settings_1_Server-Settings1.png)
2. Specify the SAP Business One database server name/address. You can take it from here:

  :::info Path

  SAP Business One → Administration → Choose Company

  :::

  ![Choose company server](./../media/choose-company-server2.png)

  Click the Add button after setting the server name:

  ![Connection](./../media/CT-License-Server-Settings_1_Server-Settings1.png)

  :::info

  Depending on a database server type, specify the server name in the below format:

  | Database Server Type                   | Server Name Format    | Example                    |
  |----------------------------------------|-----------------------|----------------------------|
  |Microsoft SQL Server, default Instance  |ServerName             |Test-System                 |
  |Microsoft SQL, Named Instance           |ServerName\InstanceName|Test System\Production      |
  |SAP HANA                                |ServerName:Port        |10.0.0.199:30015            |

  :::

  You can check the Microsoft SQL Server server/instance name using the Microsoft SQL Server Management Studio tool:

  <details>
  <summary>Click here to find out more</summary>

  ![SQL Server](./../media/sql-server-management-studio.png)

  </details>

  You can read the SAP Business One HANA server and port in the SAP Business One installation:

  <details>
  <summary>Click here to find out more</summary>

  ![HANA](./../media/hana-address.png)

  </details>
3. Set a database server type of added server:
  
  ![License Server](./../media/license-server-settings_2_db-connection_3.png)
4. Specify a database user name the password, and click Save:

  ![License Server](./../media/license-server-settings_2_db-connection_4.png)
5. If inserted data are correct, then the 'Data has been saved' message appears, and the company database number is displayed next to the server position:

  :::caution

  Please note that the application does not show a newly restored/imported company database until Choose Company list in the SAP Business One client is refreshed.

  :::

  ![Caution](./../media/license-server-settings_2_db-connection_5.png)

  :::info

  After this step, the message: ```Direct Data Access mode is disabled.``` will not appear anymore while starting ProcessForce provided that the connection to CompuTec License Server is configured correctly in ProcessForce License Administration (see [Extension → License assignment section](./../../installation/first-installation/extension).
  
  :::
6. You can also expand the list of existing company databases to make sure that a particular SAP Business One company uses Direct Data Access:

  ![Expand List](./../media/license-server-settings_2_db-connection_6.png)

  :::caution

  Please note that there is a need to refresh the list each time you add/import a new company database:

  ![Refresh](./../media/license-server-settings_2_db-connection_7.png)

  :::
7. On a highlight of the added database server position, you will not see inserted database user name and actual password length anymore due to a created mask:

  ![Mask](./../media/license-server-settings_2_db-connection_8.png)

  :::info

  It is possible to set up more than one database server:

  <details>
  <summary>Click here to find out more.</summary>

  ![Multiple databases](./../media/license-server-settings_2_db-connection_9.png)

  </details>

  :::

