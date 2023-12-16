# CompuTec License Server Release Notes

:::info

The installer is available to download from **ADD LINK** here.

:::

## CompuTec License Server 5.10.2.9

4 November 2020

### Changes and improvements

#### Server – ProcessForce, Server – PDC/WMS (terminals), Server – Labels

- Possibility to generate the license Hardware Key (CompuTec Key / Terminal License Key / Labels Hardware Key / Premium Hardware Key) on Amazon Elastic Compute Cloud (EC2) Instance in Amazon Web Services (AWS) infrastructure in case of the inability to connect to IPv4 URI by the system

#### Server – WMS

- SSL support for CompuTec WMS implemented

#### Service Manager

- COMPUTEC LICENSE SERVER SETTINGS window, Database Connection tab:

  - The database list of a saved server is now sorted alphabetically

  - Refreshing the database list of a saved server now removes non-existent databases from the displayed list (without having to reset the server entry)

### Fixes

#### Server, Service Manager

- Stopping CompuTec License Server service while opening CompuTec Service Manager windows in some cases

---

## CompuTec License Server 5.10.2.1

13 March 2020

### Changes and improvements {#01}

#### Service Manager {#02}

- COMPUTEC LICENSE SERVER SETTINGS window, Database Connection tab: database and its credentials highlighted when Database User Name or User Password is different than the one saved for the related database server in the root position

### Fixes {#fixes02}

#### Service Manager {#serve-02}

- An error on clicking the PDC/WMS Licensing button in case of permanent PDC/WMS license in some cases.

  ```
  Connection failed Error connecting to the license server
  ```

#### Server

- License Hardware Key (CompuTec Key / Terminal License Key / Labels Hardware Key / Premium Hardware Key) is not displayed in case of a problem with reading a hardware component

---

## CompuTec License Server 5.10.1.1

19 February 2020

### Changes and improvements {#03}

#### Server {#04}

- Compatibility with CompuTec solutions dedicated for SAP Business One 10.0