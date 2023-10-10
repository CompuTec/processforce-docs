# ProcessForce API Performance Tips & Tricks

Since objects like Manufacturing Orders or Bill of Materials can be massive, keeping smooth performance when operating on them can be challenging. Here you can find several recommendations regarding improving performance when working with ProcessForce API.

## Class QueryManager

Try to use our class QueryManager instead of the one provided by SAP. If there is no transaction in the proceeding, then using our QueryManager should be faster to get information from a database. If there is a transaction in the background at the moment of calling the QueryManager function, then it will call the SAP version of QueryManager, which would very often speed up the process a bit, and you do not risk anything.

## Manufacturing Order and Bill of Materials synchronization

A synchronization mechanism is implemented for objects like Manufacturing Order or Bill Of Materials. When you create or update our Manufacturing Order object, the Production Order object (SAP) is created with the same data. In the same way, when you add or update our Bill of Materials object, the SAP Business One version of this object is created. This synchronization plays a vital role in the proceeding of our objects, so you might consider turning this option off. You can find the settings responsible for that in General Settings.

But first, please try to determine if the mechanism is needed in business logic in your case.

## Direct data access

For now, it works only for reading data from the database. You must configure your license server properly to use direct data access. A detailed description of how to use it is available here. This should boost performance. If you cannot see the mentioned website, contact your CompuTec solutions provider.

## Bulk functionality

If you are reading many objects in a scenario, then you should consider using the Bulk functionality. This speeds up things in the case of using many objects (like tens or hundreds). But noticeable performance benefits can be observed even for 3 objects.
An example of how to use the Bulk functionality is below. It is required to set up a direct data access connection for this.

The following example creates a list of ManufacturingOrderItems objects that have a BillOfMaterial object from the IManufacturingOrder collection and then takes the whole list of such BOM objects at once.

Object Items in the first line contain Items object from ManufacturingOrder UDO object and function GetBomCode at the end of the second line is returning Code of Bill of Material object for the specified U_ItemCode and U_Revision.

```csharp
var list = Items.Where(p => (p as ManufacturingOrderItems).IsRowFilled() && ItemUtils.Technology.BillOfMaterialExists(Token, p.U_ItemCode, p.U_Revision));
var boms = BulkUdoConverter.GetBulkObjects<string, IBillOfMaterial>(Token, ObjectTypes.BillOfMaterial, list.Select(p => GetBomCode(Token, p.U_ItemCode, p.U_Revision)));
```

## U_ItemCode and U_Revision filling

Better performance could be obtained by changing the implementation filling of U_ItemCode and U_Revision into filling U_BOMCode and U_ReferenceDicitionary properties of Manufacturing Order UDO.

Function GetBomCode at the end of the second line returns the Code of Bill of Material object for the specified U_ItemCode and U_Revision.

Example:

```csharp
Dictionary<Tuple<string, string>, string> bomCodes = new Dictionary<Tuple<string, string>, string>();
Dictionary<string, CompuTec.ProcessForce.API.Documents.BillOfMaterials.IBillOfMaterial> ReferenceDictionary = new Dictionary<string, API.Documents.BillOfMaterials.IBillOfMaterial>();

for (int i = 0; i < howManyRows; i++)
{
	string itemcode = dataTable.GetValue("ItemCode", i);
	string revision = dataTable.GetValue("Revision", i);

	if (!string.IsNullOrEmpty(itemcode) & !string.IsNullOrEmpty(revision))
	{
		Tuple<string, string> itemCodeAndRevision = new Tuple<string, string>(itemcode, revision);
		if (!bomCodes.ContainsKey(itemCodeAndRevision))
		{
			bomCodes.Add(itemCodeAndRevision), GetBomCode(itemCodeAndRevision.Item1, itemCodeAndRevision.Item2);
		}
	}
}

var boms = BulkUdoConverter.GetBulkObjects<string, IBillOfMaterial>("", ObjectTypes.BillOfMaterial, bomCodes.Values.Distinct().ToList());
foreach(IBillOfMaterial bom in boms)
ReferenceDictionary.Add(bom.Code, bom);

for (int i = 0; i < howManyRows; i++)
{
	CompuTec.ProcessForce.API.Documents.ManufacturingOrder.ManufacturingOrder udo = UdoFactoryClass.CreateDocument(string.Empty, ObjectTypes.ManufacturingOrder);

	udo.ReferenceDictionary = ReferenceDictionary;

	//udo.U_ItemCode = itemCode;
	//if (!string.IsNullOrEmpty(revision))
	//{
	//	udo.U_Revision = revision;

	udo.U_BOMCode = bomCodes[new Tuple<string, string>(itemCode, revision)]
};

```

As you can see above, there is no need to set U_itemCode or U_Revision field. Because setting U_ItemCode is slow, we want to avoid it by setting up BOMCode and ReferenceDictionary.

Be aware that this issue is not as stable as the standard way, so it is a good idea to surround this code with the try catch and also check if there is such a BOMCode in the builded list. If there is not, then set U_ItemCode. If there is, then set BomCode.
