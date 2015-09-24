# Object Load Options (JavaScript API for Excel)

Represents an object that can be passed to the load method to specify the set of properties and relations to be loaded upon execution of sync() method that synchronizes the states between Excel objects and corresponding JavaScript proxy objects in the add-in. This takes in options such as select, expand parameters to specify set of properties to be loaded on the object and also allows pagination control on collection.

It is also valid to supply a string containing the properties and relations to be loaded or an aray containing list of properties and relations to be loaded. 

## Properties
| Property	   | Type	|Description|Notes |
|:---------------|:--------|:----------|:-----|
|select|object|Provide comma delimited list or an array of parameter/relationship names to be loaded upon executeAsync call. e.g. "property1, relationship1", [ "property1", "relationship1"]. Optional.||
|expand|object|Provide comma delimited list or an array of relationship names to be loaded upon executeAsync call. e.g. "relationship1, relationship2", [ "relationship1", "relationship2"]. Optional.||
|top|int| Specify the number of items in the queried collection to be included in the result. Optional.||
|skip|int|Specify the number of items in the collection that are to be skipped and not included in the result. If `top` is specified, the selection of result will start after skipping the specified number if items. Optional.||

#### Examples

Below example select the top 100 rows of table.

```js
Excel.run(function (ctx) { 
	var table = ctx.workbook.tables.getItem("Table1");
	var tableRows = table.rows.load({"select" : "index, values","top": 100, "skip": 0 })
	return ctx.sync().then(function() {
		for (var i = 0; i < tableRows.items.length; i++)
		{
			console.log(tableRows.items[i].index);
			console.log(tableRows.items[i].values);
		}
	});
}).catch(function(error) {
		console.log("Error: " + error);
		if (error instanceof OfficeExtension.Error) {
			console.log("Debug info: " + JSON.stringify(error.debugInfo));
		}
})
```