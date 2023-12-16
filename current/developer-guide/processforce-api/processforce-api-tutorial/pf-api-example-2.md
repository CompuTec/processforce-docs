# Example 2: Creating simple objects

How to add a simple object using the ProcessForce API

- All ProcessForce documents must be created using the factory method in IProcessForceCompanyObject called CreatePFObject.

- All objects contain methods Add, Update, and GetByKey that can be used for adding, updating, or retrieving objects from the database.

- All document objects can be found in CompuTec.ProcessForce.API.Documents namespace.

*The example below illustrates how to create a Resource Group object*

```csharp
try
            {
                //Create Resource Group Object
                IResourceGroup resgrp = company.CreatePFObject(CompuTec.ProcessForce.API.Core.ObjectTypes.ResourceGroup);
                resgrp.U_RscGrpCode = "GrpCode";
                resgrp.U_RscGrpName = "GrpName";
                if (resgrp.Add() == 0)
                    return true;
            }
            catch (UDOException e)
            {
                Console.WriteLine(string.Format("Error while adding { } object Message:{1}", e.ObjectCode, e.Message));
            }
            catch (Exception e)
            {
                Console.WriteLine(string.Format("Error while adding", e.Message));
            }
```
