## Accessing Data with IBADI IB_runProc function

IB_runProc is one of the most important fuctions in UBADI. It is the gateway to the database objects (tables, stored procedures, functions etc). It is used for calling stored procedures.  When you carry out actions like Delete and Update you can simply call IB_runProc, giving it the name of the stored procedure and the values of the parameters, without requiring any output. 
e.g. 
```
   var params ={Descrription:"Cash given to me by Dad",Category:"Pocket money",Credit:10.00,Debit:0};
   IB_runProc("usp_update_trans",params);
```   

However if you want to return a resultset, you need to catch the result
e.g.
```
   var arr = IB_runProc("usp_get_trans_data",{});
```

Lets use data in the AdventureWorks database for some examples:

1. Create a stored procedure for fetching a list of countries/regions in the Adventureworks database called  ibadi.usp_getcountryregion_data.  

```
CREATE PROC ibadi.usp_getcountryregion_data
AS
BEGIN
   SELECT DISTINCT CountryRegionName 
   FROM Person.vStateProvinceCountryRegion
END
```

If you run this in your SSMS you get something like this:

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Simple-example.PNG?raw=true%22"/>


Now lets run IB_runProc and see what it looks like:
```sql
CREATE PROCEDURE [webpage].[Getting Data with IB_runProc_1]
AS
BEGIN
 DECLARE @html varchar(max)  
 SELECT  @html=  
	'  
	<div id="dvOutput" style="padding: 3px; width: 100%; word-break: break-all; word-wrap: break-word;"/>
	<script>
	  var result = IB_runProc("ibadi.usp_getcountry_data",{});
	  $("#dvOutput").append($("<span>"+JSON.stringify(result)+"</span>"));
	</script>
	'
 SELECT html = [ibadi].[html](@html)     
END
```

Use the following URL to view this new page.

http://localhost:64802/index.html?page_name=Getting%20Data%20with%20IB_runProc_1

...and you will see something like this:

[[{"CountryRegionName":"American Samoa"},{"CountryRegionName":"Australia"},{"CountryRegionName":"Canada"},{"CountryRegionName":"Germany"},{"CountryRegionName":"Micronesia"},{"CountryRegionName":"France"},{"CountryRegionName":"United Kingdom"},{"CountryRegionName":"Marshall Islands"},{"CountryRegionName":"Northern Mariana Islands"},{"CountryRegionName":"Palau"},{"CountryRegionName":"United States"},{"CountryRegionName":"Virgin Islands, U.S."}]]


Note that the output from IB_runProc is expected to be an array of datasets(which themselves are arrays of cols X rows)
e.g. Output containing 3 datasets
```
[
    //Dataset 1
	[{
		col1: val1,
		col2: val1,
		col3: val1
	}, {
		col1: val2,
		col2: val2,
		col3: val2
	}, {
		col1: val3,
		col2: val3,
		col3: val3
	}],
	
	//Dataset 2
	[{
		col1: val1,
		col2: val1,
		col3: val1
	}, {
		col1: val2,
		col2: val2,
		col3: val2
	}],
	
	//Dataset 3
	[{
		col1: val1,
		col2: val1,
		col3: val1
	}, {
		col1: val2,
		col2: val2,
		col3: val2
	}, {
		col1: val3,
		col2: val3,
		col3: val3
	}, {
		col1: val4,
		col2: val4,
		col3: val4
	}]
]
```

e.g. Output containing a single dataset consisting of a table of one column and a single row.
```
[[col1:val1]]
```

You would normally use the output from IB_runProc to populate a table, list boxes, combo boxes etc or other actions that require data drom the database to be utilised or displayed. For instance in the credit tracking tool, we had the following:

```javascript
var arr = IB_runProc("usp_get_trans_data",{});
arr = arr[0]; /*Using only the first dataset of the output from IB_runProc*/

var tr_str="";  
$("#PlaceholderForCurrentBalance").text(arr[0].Balance);
var current_row_template = "<tr><td>$Date$</td><td>$Descrription$</td><td>$Category$</td><td>$credit$</td><th>$debit$</td><td>$balance$</td></tr>";
for (var i = 0, len = arr.length; i < len; i++) {         
var itm = arr[i];
var current_row = current_row_template.replace("$Date$",itm.Date);
 current_row = current_row.replace("$Descrription$",itm.Descrription);
 current_row = current_row.replace("$Category$",itm.Category);
 current_row = current_row.replace("$credit$",itm.Credit);
 current_row = current_row.replace("$debit$",itm.Debit);
 current_row = current_row.replace("$balance$",itm.Balance);
 tr_str = tr_str + current_row;
}

var table_obj =$(tr_str); 
$("#tableData").empty();
$("#tableData").append(table_obj); 
}
```

Here are some further examples:

### Retrieve a single value with data from IB_runProc

Get the total number of employees in the AdventureWorks database.

#### 1. Create the stored procedure.
```
CREATE PROC ibadi.usp_getTotalEmployeeCount
AS
BEGIN
	SELECT TotalEmployeeCount  = COUNT(*) FROM HumanResources.Employee
END
```

#### 2. Write the HTML, Javascript for displaying the data on a page.
```
CREATE PROC webpage.[Get Total Employee Count]
AS
BEGIN
  DECLARE @html varchar(max)=
  '
	<h3>Get Total Employee Count</h3>
	<div id="dvGetTotalEmployeeCount"/>
	<script>
	   var result = IB_runProc("ibadi.usp_getTotalEmployeeCount",{});
	   console.log(result);
	   $("#dvGetTotalEmployeeCount").append("<span>Total employee count = "+result[0][0].TotalEmployeeCount+"</span>");
	</script>
  '
  SELECT html=[ibadi].[html](@html)  
END
```

#### 3. And the result is:
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/example_get_total_count.PNG?raw=true"/>

### Populate a listbox or dropdown with data from IB_runProc

In this example we will pull a list of countries/regions from the AdventureWorks database and use it to populate a <select> object which forms a list or dropdown input box


#### 1. Create the stored procedure.
```
CREATE PROC ibadi.usp_getcountryregion_data
AS
BEGIN
   SELECT DISTINCT CountryRegionName 
   FROM Person.vStateProvinceCountryRegion
END
```
	
#### 2. Write the HTML, Javascript for displaying the data on a page.

```
CREATE PROC webpage.[Get Countryregion List]
AS
BEGIN
  DECLARE @html varchar(max)=
  '
	<h3>Country/Region Listbox</h3>
	<div id="dvListBox"/>

	<h3>Country/Region Drop-down</h3>
	<div id="dvDropdown"/>

	<script>
	   var result  = IB_runProc("ibadi.usp_getcountryregion_data",{});
	   var dataset = result[0];

	   /* ... Drop-down ... */
	   var select  = $("<select>");
	   select.append($("<option>Select something...</option>")); 
	   dataset.forEach(function(item) {
			select.append($("<option>"+item.CountryRegionName+"</option>")); 
	   });
	   $("#dvDropdown").append(select);


	   /* ... Listbox ... */
	   var select  = $("<select multiple=multiple size=10>");
	   select.append($("<option>Select something...</option>")); 
	   dataset.forEach(function(item) {
			select.append($("<option>"+item.CountryRegionName+"</option>")); 
	   });
	   $("#dvListBox").append(select);

	</script>
  '
  SELECT html=[ibadi].[html](@html)  
END
```

#### 3. And the result is:
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/example_list_dropdown.PNG?raw=true"/>

### Populate HTML tables with datasets from IB_runProc
In this example we will use a stored procedure that returns more than one dataset when it is called with IB_runProc. In order to do this we will use a javascript function that can render any given dataset as an html table. 
The datasets are: 
```
1. A summary of all employees split by gender.
2. list of departments and the number of employees in it.
3. List of product categories and the number of products in each. 
4. YTD Sales for each territory for 2018.
```

#### 1. Create the stored procedure.
```
CREATE proc [ibadi].usp_employee_dept_production_data
AS
BEGIN
   SET NOCOUNT ON
		--1. A summary of all employees split by gender.
		SELECT 
			  Gender,
			  [Number_of_employees]=count(*)
		FROM  HumanResources.Employee
		GROUP BY Gender

		--2. list of department group and the number of employees in it.
		select 
			 D.GroupName,  
			 [Number_of_employees]=count(*)
		from HumanResources.Employee E
		inner join HumanResources.EmployeeDepartmentHistory EDH
		on E.BusinessEntityID = EDH.BusinessEntityID
		inner join HumanResources.Department D
		on D.DepartmentID = EDH.DepartmentID and E.BusinessEntityID = EDH.BusinessEntityID
		where EDH.StartDate = (select max(StartDate) from HumanResources.EmployeeDepartmentHistory where BusinessEntityID=EDH.BusinessEntityID)
		group by D.GroupName

		-- 3. List of product categories and the number of products in each.
		SELECT 
			  Subcategory=ISNULL(PC.Name,'Not Categorised')
			, [Nunmber_of_products]=count(*) 
		FROM Production.ProductCategory PC
		inner join Production.ProductSubcategory PSC
		ON PC.ProductCategoryID = PSC.ProductCategoryID
		right join Production.Product P
		ON  P.ProductSubcategoryID = PSC.ProductSubCategoryID
		GROUP BY ISNULL(PC.Name,'Not Categorised')

		-- 4. YTD Sales for each territory for 2008 and 2007.
		SELECT   [Region/group]=[Group] 
				,[SalesYTD]=sum([SalesYTD])
				,SalesLastYear=sum(SalesLastYear)
		FROM    sales.SalesTerritory
		group   by [Group]
END
```

#### 2. Write the HTML, Javascript for displaying the data on a page.

```SQL
CREATE PROCEDURE [webpage].[Populate HTML tables with datasets]
AS
BEGIN 
DECLARE @html varchar(max)  
SELECT  @html=  
'  
<div>
  <h3>Populating HTML tables with datasets</h3>
</div>
<div>   
  <div id="test_data_0" class="box2"><span>Employees split by gender.</span></div>
  <div id="test_data_1" class="box2"><span>Departments group and the number of employees in each.</span></div>
  <div id="test_data_2" class="box2"><span>Product categories and the number of products in each.</span></div>
  <div id="test_data_3" class="box2"><span>YTD Sales for each territory for 2008 and 2007.</span></div>
</div>
<style>
.box2 {
  display: inline-block;
  width: 300px;
  height: 100px;
  margin: 1em;
}
tr:hover {background-color: #f5f5f5;}
tr:nth-child(even) {background-color: #f2f2f2;}
th {
  background-color: #4CAF50;
  color: white;
}
th, td {
  border-bottom: 1px solid #ddd;
}
span {
  font-size:12px;
}

</style>
<script>
var result = IB_runProc("[ibadi].usp_employee_dept_production_data",{});

if ( Object.prototype.toString.call( result ) === "[object Array]") {
	/*object is OK"*/
	if (result.length > 0)
	{
	  renderTable("test_data_0", result[0]);
	}

	if (result.length > 1)
	{
	  renderTable("test_data_1", result[1]);
	}

	if (result.length > 2)
	{
	  renderTable("test_data_2", result[2]);
	}

	if (result.length > 3)
	{
	  renderTable("test_data_3", result[3]);
	}
} else{
	 /*object is NOT OK*/
	 $("#test_data_0").append("<b>UNABLE TO READ DATA</b><p>"+result.error+"</p>")
};

function renderTable(div_id, dataset_array)
{
  var headerItems = Object.getOwnPropertyNames(dataset_array[0]);
  var tb = document.createElement("table");
  var tr = document.createElement("tr"); 
  headerItems.forEach(function(headerelement) {
	  var th = document.createElement("th");
	  th.appendChild(document.createTextNode(headerelement)); 
	  tr.appendChild(th);
  });
  tb.appendChild(tr);

  dataset_array.forEach(function(element) {
	  var tr = document.createElement("tr");
	  headerItems.forEach(function(headerelement) {
		 var td = document.createElement("td");
		 td.appendChild(document.createTextNode(element[headerelement])); 
		 tr.appendChild(td);
	  });
	  tb.appendChild(tr);
  });

  document.getElementById(div_id).appendChild(tb);
}
</script>
'
SELECT html = [ibadi].[html](@html)     
END
```


#### 3. And the result is:
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/example_Populate_HTML_tables_with_datasets.PNG?raw=true"/>


  





