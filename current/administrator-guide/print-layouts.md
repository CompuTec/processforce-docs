---
sidebar_position: 8
---

# Print Layouts

On this page, you can find corrected ProcessForce print layout files.

---

Apart from bug fixes and visual adjustment of the files, versions destined for SAP Business One HANA have been added too.

:::info
    Subsequent print layout files and updates of the current ones will be posted on this page until they are incorporated into the ProcessForce installer.
:::

:::info
    If you notice that any report from the list below should be corrected, do not hesitate to create an issue in the [http://support.computec.pl](http://support.computec.pl/) portal (Support ProcessForce project) with the Print Layouts or Reports component.
:::

:::danger
    If you want to adjust any of the layout and report files, please make sure to make a copy of it. The default ones (with ProcessForce in their names) may be overwritten during future updates, and changes in them may be lost.
:::

All reports are in the English version.

## Routing

- Name: **Routing**
- Type: Print Layout
- File Version: 2.43
- Content:
  - Routing:
    - header details
    - Operation details
    - Operation Properties (table)
    - Operation Resource details
    - Operation Resource times (table)
    - Operation Resource Properties (table)
- Example: Preview <!-- TODO: Add Link -->
- Remarks:
  - Values:
    - Value, To Value (Operations Properties table),
    - Queue Time, Setup Time, Run Time, and Stock Time (Resource Times table)
  - must have a limited length for displaying them correctly on the report.
- Version for Microsoft SQL Server: Download <!-- TODO: Add Link -->
- Version for SAP HANA: Download <!-- TODO: Add Link -->

## Bill of Materials

- Name: **Bill of Materials**
- Type: Print Layout
- File Version: 2.13
- Content:
  - Bill of Materials:
    - header details
    - Revision details
    - Items (table)
    - Coproducts (table)
    - Scraps (table)
    - WIP Items (table)
    - Routing details
    - Routing Operation details
    - Routing Operation Properties (table)
    - Routing Operation Resource details
    - Routing Operation Resource times (table)
    - Routing Operation Resource Properties (table)
- Remarks:
  - Displaying / not displaying inactive Routings (parameter)
  - Displaying / not displaying not default Resources (parameter)
- Version for Microsoft SQL Server: Download <!-- TODO: Add Link -->
- Version for SAP HANA: Download <!-- TODO: Add Link -->

:::warning Reports in version for SAP HANA
    To run a report in SAP Business One, a version for SAP HANA, there is a need to execute the attached SQL procedure/s on the applicable database.
:::
