# Example 1: Connecting to database

How to connect to the database using the ProcessForce API:

- Add a reference of Computec.ProcessForce.API.dll and SAPbobsCOM.dll.
- After installing ProcessForce on your computer, you will find the Computec.ProcessForce.API.dll. reference within the .NET references folder.
- The base object in the API is IProcessForceCompany, which can be found within Computec.ProcessForce.API namespace allows connecting to the database creating SAP and ProcessForce objects.
- It must be created using ProcessForceCompanyInitializator.CreateCompany();

```csharp
IProcessForceCompany company = ProcessForceCompanyInitializator.CreateCompany();
            try
            {
                //set connection constaint
                company.SQLServer = "PCNBPO02";
                company.UseTrusted = true;
                company.UserName = "manager";
                company.Password = "enigma";
                company.LicenseServer = "127.0.0.1";
                company.DbServerType = SAPbobsCOM.BoDataServerTypes.dst_MSSQL2008;
                company.Databasename = "PFDemo";
                company.Language = SAPbobsCOM.BoSuppLangs.ln_Polish; 
                //Connect to Company
                if (company.Connect() == 1) 
                    Console.WriteLine(string.Format("Connected to Company {0}", company.SapCompany.CompanyName)); 
                else
                    Console.WriteLine(string.Format("Not Connected To compmany Error:{0}", company.getLastErrorDescription()));
            }
            catch (Exception ex)
            {
                Console.WriteLine(string.Format("Exception was throw {0}", ex.Message ));
            }
            finally
            {

                Console.ReadKey();
                if(company.IsConnected)
                    company.Disconnect();
            } 
```
