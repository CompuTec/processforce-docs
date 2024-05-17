---
sidebar_position: 6
---

# Example 5 Creating Actions

## How to work with Actions using ProcessForce API

An action is an object which gives you the possibility to:

- Create Pick Orders for one or more Manufacturing Orders
- Create Pick Receipts for one or more Manufacturing Orders
- Create Production Receipts/Issues on assigned Pick Order
- Create Correction of Production Issues/Receipts.

1. **Create Pick Receipt for Manufacturing Order**:

    ```csharp
    ICreatePickReceiptForProductionReceipt prAction = company.CreatePFAction(CompuTec.ProcessForce.API.Core.ActionType.CreatePickReceiptForProductionReceipt);
    //You can add several MOR docentrys
    prAction.AddManufacturingOrderDocEntry(1);
    //Choose Receipt Type {All;Scraps;Coproducts;FinalGood}
    prAction.ReceiptType = PickOrderdReceiptType.All;
    object pickReceiptDocEntry;
    //Fires Action
    if (prAction.DoAction(out pickReceiptDocEntry))
    {
        Console.WriteLine("Pick Receipt created " + pickReceiptDocEntry.ToString());
    }
    ```

2. **Create Pick Order for Manufacturing Order**:

    ```csharp
    ICreatePickOrderForProductionIssue poAction = company.CreatePFAction(CompuTec.ProcessForce.API.Core.ActionType.CreatePickOrderForProductionIssue);
    //You can add several MOR docentrys
    poAction.AddMORDocEntry(1);
    object pickOrderDocEntry;
    //Fires Action
    if (poAction.DoAction(out pickOrderDocEntry))
    {
        Console.WriteLine("Pick Order created " + pickOrderDocEntry.ToString());
    }
    ```

3. **Create Production Goods Issue**:

    ```csharp
    ICreateGoodsIssueFromPickOrderBasedOnProductionIssue prodissue = company.CreatePFAction(CompuTec.ProcessForce.API.Core.ActionType.CreateGoodsIssueFromPickOrderBasedOnProductionIssue);
    prodissue.DocDate = DateTime.Today;
    prodissue.TaxDate = DateTime.Today;
    prodissue.PickOrderID = 1;
    object goodsIssueDocEntry;
    prodissue.DoAction(out goodsIssueDocEntry);
    ```

4. **Create Production Goods Receipt**:

    ```csharp
    ICreateGoodsReceiptFromPickReceiptBasedOnProductionReceipt prodeceipt = company.CreatePFAction(CompuTec.ProcessForce.API.Core.ActionType.CreateGoodsReceiptFromPickReceiptBasedOnProductionReceipt);
    prodeceipt.DocDate = DateTime.Today;
    prodeceipt.TaxDate = DateTime.Today;
    prodeceipt.PickReceiptID = 2;
    object goodsreceiptDocentry;
    prodeceipt.DoAction(out goodsreceiptDocentry);
    ```

    For Production Goods Issue and Receipt, you can also specify documents series, memos, and other information, if you do not wish to use default values.
