# Extension

## Installation Guide

On this page, you can find a step-by-step guide on how to install the ProcessForce extension.

---

:::note

If you only need to upgrade ProcessForce, please check [the ProcessForce Upgrade Guide](../upgrade).

:::

:::caution

Be sure to acquaint yourself with [the ProcessForce System Requirements](../../system-requirements.md) before starting the installation

:::

:::caution

It is recommended to install [CompuTec License Server](./license-server) first.

:::

:::caution

It is recommended to restart the SAP Business One client before the installation of the ProcessForce extension.

:::

:::caution

Due to technical limitations, it is required to execute a specific SQL query on the company database during the first installation of the extension in the SAP HANA version. More details you can find in the 8. step of the installation description.

:::

:::danger

Before the ProcessForce extension upgrade, the following SAP Business One stored procedures are required to be in a default form (do not contain any custom queries):

- SP_TransactionNotification
- SP_PostTransactionNotice

:::

:::danger

Before starting the installation, ensure that Serial Numbers and Batches, Production, and Units of Measure boxes are **unchecked** in General Settings → **Hide functions** (the way it is on the screenshot provided below).

<details>
  <summary>Screenshot</summary>
  
  <p align="center">

![Checkboxes](./media/general-settings-unchecked.webp)

  </p>

</details>

:::

:::danger

Please note that Copy Express, or the option Copy User-Defined Fields and Tables/Copy User-Defined Objects in the Create New Company wizard, cannot be used for copying ProcessForce objects and structures.

<details>
  <summary>Click here to find out more.</summary>

When a new ProcessForce database is created by using Copy Express, or with the option Copy User-Defined Fields and Tables/Copy User-Defined Objects in the Create New Company wizard, the SAP Business One mechanism does not set a proper value for EditType fields in Processforce structures (it is set to NULL). It is a result of an SAP Business One bug: new fields with DateTime type are not properly created in a new database.

  </details>

To avoid this situation, it is possible to create a new database, install ProcessForce and move the required data through ProcessForce API with [PowerShell scripts](../../../developer-guide/data-import/data-import.md).

:::

### Installation

:::danger

Please note that ProcessForce in 10 version is provided as a **Lightweight Deployment Extension only**. Please use **SAP Business One Extension Manager** to install it.

:::

Download installation files for it here.**ADD-LINK**

ProcessForce for SAP Business One 10.0 is a lightweight deployment extension. As such to install it, log in to your SLD Extension service and import and assign the ProcessForce extension from there.

1. You can access it directly by using a default URL `HTTPS://<SERVER_NAME>:40000/ExtensionManager` or by opening it from your SAP Business One client. Choose the highlighted option in SAP Business One:

![Add-on Administration](./media/addon-administration-extension.webp) 2. Log in to System Landscape Directory. 3. Click the Import button and browse to the path with the extension installation file:

![Import add-on](./media/import-add-on.webp) 4. Go to the Company Assignment tab, choose the required database from the Company List and click the Assign button:

![Assing a database](./media/assign-database.webp) 5. Choose the ProcessForce option and go through the installation by clicking Next.

![Assign ProcessForce](./media/assign-processforce.webp) 6. It is recommended to set up the manual type of Startup Mode on the first run of the application. It can be changed later to Mandatory. After that click Next. 7. Once it is done, re-log your company, run the extension manually and wait until the installation is finished.

:::caution

Note that the first ProcessForce installation on a company where it was not installed previously may take several dozen minutes up to a few hours (depending on the system environment).

::: 8. If you install the ProcessForce extension for SAP HANA, you have to create manually a specific SQL procedure during the installation process. For this purpose, click the Copy to Clip... button on the displayed System Message, paste the query into SAP HANA Studio and execute it on the corresponding schema (company database):

![System Message - Procedure](./media/system-message-procedure.webp)

:::caution Common query problem

    <details>

    When you do not select a schema by opening SQL Console from the root tree schema selection, the query window is set to the default selected schema, e.g. SYSTEM schema. The first execution of a query creates a procedure in the SYSTEM schema and not in the proper company schema where ProcessForce is installed.

    To install the copied SQL procedure to the proper company schema, please open SQL Console and use the below command before the copied query to switch to the proper schema:

    ```sql
    SET SCHEMA "<COMPANY-DATABASE-SCHEMA-NAME>";
    ```

  </details>

::: 9. After the installation is completed, the message about the database structure modification is displayed – click Yes. 10. Additional message – recommending the SAP Business One client restart – is displayed: click OK. 11. At the end of the installation process the ProcessForce main menu positions appear:

![ProcessForce menu](./media/processforce%20menu.webp)

### License assignment

:::info

This step may be skipped in case of making the extension upgrade only.

:::

You can find the License Import and Assignment guide [here](./../../licensing/license-import-assignment.md)

### Data preparation

:::note

Before running restore procedures please decide if [the Manage Item Cost per Warehouse](./../../../user-guide/costing-material-and-resources/configuration/configuration.md) setting is used – if you change it after the restore, you need to remove all Item Costing Details and restore them again.

:::

If ProcessForce is installed on a database with existing data (Item Master Data entries) for the first time, it is necessary to run the data restoring procedures:

- Restore Item Details
- Restore Item Costing
- Restore Resource Costing
- Restore Employee Calendars
- Restore Batch Details

![Restore](./media/restore.webp)

Click [here](./../../../user-guide/system-initialzation/data-restore.md) to find out more about the Restore function.
