# Data import and update using PowerShell scripts

:::caution

On the 4th of March 2022, **ADD LINK** PFI (ProcessForce Import) plugin for AppEngine was released. PFI allows importing/updating data to ProcessForce objects. The idea behind this plugin is to be a simplifier, swifter, and more adjustable alternative for PowerShell updates.

:::

You can find a step-by-step manual on how to use PowerShell scripts on this page. The example used in this tutorial is for importing/updating Ingredient Classification Certificates in Item Ingredients Master Data.

:::danger

If you take our scripts and modify them, which then causes an issue of any sort which requires our team to spend time reviewing your scripts etc., we will charge you for the time spent in fixing these issues on a time and material basis at an hourly rate of 150 EUR/USD, plus expenses if applicable. Before any work is started, an email confirmation is required between both parties that these charges will be applied and accepted.

:::

:::info

Please note that 22.10.2018 updated and standardized scripts have been released. Also, with this update, connection parameters have been moved to a separate file: configuration.xml (which can be found in a zip file of a specific script).

:::

## Required files

You can use predefined PowerShell scripts. The latest versions are available from [our public git repository](https://github.com/CompuTec/processforce-powershell).

Download:

![Download](./media/scripts-download.webp)

In the following example, Ingredient Master Data scripts are used. Go to the downloaded folder and find the Ingredient Master Data folder ([...]\Inventory\Ingredients\Ingredients Master Data):

![Ingredients](./media/powershell-scripts-ingredients.webp)

Each folder contains three types of files:

- configuration file – a file that holds information on a database access

- PowerShell script – for executing import or update of the data

- comma-separated values (CSV) files – files to be filled in with data to be imported or updated

## Procedure

### Configuration file

Open the file in the edition mode using any app suitable for editing XML files:

```xml

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<configuration>
  <connection
    Database="PFDEMOGB"
    SQLServer="NDB@hanaserver:30013"
    LicenseServer="hanaserver:40000"
    DbServerType="dst_HANADB"
    UserName="manager"
    Password="1234"
  />
  <!-- Database       = Database/schema name (check in SAP Company select form/window, or in Microsoft SQL Server Management Studio or in SAP HANA Studio) -->
  <!-- SQLServer      = Database server name or IP address with port number; Should be the same as in the System Landscape Directory (check https://<Server>:<Port>/ControlCenter); Sometimes the best is to use an IP address to resolve connection problems -->
  <!-- LicenseServer  = SAP Business One Licence Server name or IP address with a port number (check in SAP Business One client -> Administration -> License -> License Administration -> License Server) -->
  <!-- DbServerType   =
                        [SAPbobsCOM.BoDataServerTypes]::"dst_HANADB"      # For SAP HANA
                        [SAPbobsCOM.BoDataServerTypes]::"dst_MSSQL2019"   # For Microsoft SQL Server 2019
                        [SAPbobsCOM.BoDataServerTypes]::"dst_MSSQL2016"   # For Microsoft SQL Server 2016
                        [SAPbobsCOM.BoDataServerTypes]::"dst_MSSQL2014"   # For Microsoft SQL Server 2014
                        [SAPbobsCOM.BoDataServerTypes]::"dst_MSSQL2012"   # For Microsoft SQL Server 2012
  -->
  <!-- UserName       = SAP Business One username, e.g., manager -->
  <!-- Password       = SAP Business One user password -->
</configuration>

```

Edit the parameters in the connection element based on the comment in the file and save the file.

The filled-in file can now be used with any of the predefined PowerShell scripts to update/import data to the database defined in the file – copy it to any of the scrip folders and replace the configuration file that is already there.

### CSV files

Set required values in the CSV files (in this example Ingredient Classification Certificates) using a suitable application (e.g., Microsoft Excel). In this example, the second and the third lines are filled with data (the first line holds the fields' names. This line is provided in the CSV file.

| IngredientCode | ClassificationCode | BusinessPartnerCode | CertificateNumber | CertificateDate | Status | StatusDate | Attachment  | Remarks |
| -------------- | ------------------ | ------------------- | ----------------- | --------------- | ------ | ---------- | ----------- | ------- |
| 2              | 2                  | 2                   | 1                 | 02.07.2015      | A      | 02.07.2015 | C:\test.pdf | remarks |
| 2              | 2                  | 2                   | 2                 | 02.07.2015      | P      | 02.07.2015 |             | remarks |

After saving changes, open it in a text editor (e.g., Notepad) and check if values are separated by a semicolon (like in the screenshot below). Sometimes, semicolons are automatically changed for a comma or other signs. This change can influence the data import/update process.

```IngredientCode;ClassificationCode;BusinessPartnerCode;CertificateNumber;CertificateDate;Status;StatusDate;Attachment;Remarks

2;2;2;1;02.07.2015;A;02.07.2015;C:\test.pdf;remarks

2;2;2;2;02.07.2015;P;02.07.2015;;remarks

```

### Executing script

Open the required script in a suitable application ( PowerShell ISE in this example). Remember to keep all the files related to one kind of data (configuration file, CSV files, script) in one folder (like in the screenshot in the required files section.

![Run script](./media/run-script.webp)
