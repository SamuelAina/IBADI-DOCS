---
title: Implementing a simple credit tracking tool with IBADI
---
## Implementing a simple credit tracking tool with IBADI

Our first IBADI project is a simple one-page application for tracking your money in a personal money account. The idea is like maintaining an electronic wallet. You can enter amount as a credit or a debit. With each entry you specify a description and the category of the transaction. The application keeps an overall balance which is displayed at the top of the page.  For audit purposes, you cannot delete any of the entries, however you can enter an equivalent negative amount and categorise that as a reversal.
Here’s a mock-up of the application:

![credit_tracking_tool_mockup](https://github.com/SamuelAina/IBADI-DOCS/blob/master/images/credit_tracking_tool_mockup.png?raw=true)

Remember that the whole idea of IBADI is to implement the application in the database, with a main stored procedure serving as the webpage. So, basically all your programming will be happening in the database in SSMS. 

The main stored procedure will be called “credit tracker” hence the url will be something like: 

`http://localhost:60382/index.html?page_name=credit tracker`

Apart from the main stored procedure, the following three database objects are used in this project:

**1. Table** -- tb_transaction – A table to store all the transactions.

```sql
create table tb_transaction
(
   [id]                  int not null identity primary key
  ,[Date]                datetime
  ,[Descrription]        varchar(255) 
  ,[Category]            varchar(80)
  ,[Credit]              money
  ,[Debit]               money 
  ,[Balance]             money
)
```

**2. Stored proc** -- usp_get_trans_data -- A stored procedure for fetching data from the table.

```sql
ALTER PROC usp_get_trans_data
AS
BEGIN
    SELECT 
        [id]	
	   ,[Date]	  = CONVERT(VARCHAR(20), [Date], 106) + ' ' 
	              + substring(CONVERT(VARCHAR(20), [Date], 120),12,8) 
	   ,[Descrription]	
	   ,[Category]	
	   ,[Credit]  = CONVERT(varchar, CAST([Credit] AS money), 1)	
	   ,[Debit]	  = CONVERT(varchar, CAST([Debit] AS money), 1)	
	   ,[Balance] = CONVERT(varchar, 
	         CAST(
	        (
				SELECT sum([Credit]-[Debit]) 
				from tb_transaction 
				where [date] <= T.[date]) AS money
			), 1)		 
	FROM tb_transaction T
	ORDER BY ID DESC
END
```

**3. Stored proc** -- usp_update_trans – A stored procedure for adding or updating entries in the table.

```sql
alter proc usp_update_trans
   @Descrription        varchar(255) 
  ,@Category            varchar(80)
  ,@Credit              money
  ,@Debit               money 
AS
BEGIN
  DECLARE  @Balance  MONEY

  INSERT tb_transaction
  (
   [Date]                
  ,[Descrription]       
  ,[Category]           
  ,[Credit]              
  ,[Debit]                
  ,[Balance]               
  )
  VALUES
  (
   getdate()               
  ,@Descrription        
  ,@Category         
  ,@Credit              
  ,@Debit                
  ,@Balance              
  )
  SELECT result='OK'
END
GO

```

The page stored procedure is listed below.

```sql
ALTER proc webpage.[credit tracker]
AS
BEGIN
 DECLARE @html varchar(max)  
 SELECT  @html=  
 '  
   <meta name="viewport" content="width=device-width, initial-scale=1">
   <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
   <div class="w3-container w3-teal"> 
   <h3>Credit/Debit Tracking Tool</h3>
   </div>

   <div class="w3-container"> 

   <div class="w3-panel w3-leftbar w3-pale-yellow w3-border-yellow">
   Current balance is <b>£<span id="PlaceholderForCurrentBalance"></span></b>
   </div>

   <div class="w3-container w3-green w3-half">
   <fieldset>
      <label>Add credit:</label><input  class="w3-input"  id="creditAmount" type="text" value="0.00"/>
	  <label>Description:</label><input class="w3-input"  id="creditdesc"   type="text" value=""/>
	  <label>Category:</label>
	  <select class="w3-select"  id="creditcategory">
	  	  <option>(none)</option>
	  	  <option>Salary</option>
	  	  <option>Casual reward</option>
	  	  <option>Windfall</option>
	  	  <option>Pocket money</option>
	  	  <option>Govenment grant</option>
	  	  <option>Investment dividend</option>
	  	  <option>Inheritance</option>
	  	  <option>Purchase advance</option>
	  	  <option>Compensation</option>
	  	  <option>Good behavour bonus</option>
	  	  <option>Emergency Fund</option>		  
	  	  <option>Tax rebate</option>		  
	  	  <option>REVERSAL</option>	
	  </select>
	  <button class="w3-button w3-black w3-right"  onclick="addCredit()">Add</button>
   </fieldset>
   </div>

   <div  class="w3-container w3-red w3-half">
   <fieldset>
      <label>Deduct debit:</label><input class="w3-input" id="debitAmount" type="text" value="0.00"/>
	  <label>Description:</label><input  class="w3-input" id="debitdesc"   type="text" value=""/>
	  <label>Category:</label>
	  <select class="w3-select" id="debitcategory">
	      <option>(none)</option>
	  	  <option>Income</option>
	  	  <option>Housing</option>
	  	  <option>Transport</option>
	  	  <option>Savings</option>
	  	  <option>Utilities</option>
	  	  <option>Health Care</option>
	  	  <option>Food and Groceries</option>
	  	  <option>Personal Care</option>
	  	  <option>Entertainment</option>
	  	  <option>Charity</option>	
	  	  <option>Gift</option>
	  	  <option>REVERSAL</option>
	  </select>
	  <button  class="w3-button w3-black w3-right" onclick="deductCredit()">Deduct</button>
   </fieldset>
   </div>
   <div>
   <h3>Transaction history</h3>
	</div>
      <table  class=''w3-table w3-striped w3-bordered w3-border w3-hoverable''>
        <thead><tr class=''w3-dark-grey''><th>Date/Time</th><th>Descrription</th><th>Category</th><th>Credit (£)</th><th>Debit (£)</th><th>Balance (£)</th></tr> </thead>
		<tbody id="tableData"></tbody>
      </table>
   </div>

<script>
   function addCredit()
   {
    var creditdesc = $("#creditdesc").val();
	var creditAmount = $("#creditAmount").val();
	var creditcategory = $("#creditcategory").val(); 
    
	if(creditdesc.trim()==""){
	  alert("Comment is required!");
	  return;
	}

	if(creditAmount.trim()=="0.00"){
	  alert("Creadit value is required!");
	  return;
	}
	var params ={Descrription:creditdesc,Category:creditcategory,Credit:creditAmount,Debit:0};
	var result = IB_runProc("usp_update_trans",params);
	$("#creditcategory").val("(none)");
	$("#creditAmount").val("0.00");
	$("#creditdesc").val("");
	refreshTable();
   }

   function deductCredit()
   {
    var debitdesc = $("#debitdesc").val();
	var debitAmount = $("#debitAmount").val();
	var debitcategory = $("#debitcategory").val(); 

	if(debitdesc.trim()==""){
	  alert("Comment is required!");
	  return;
	}

	if(debitAmount.trim()=="0.00"){
	  alert("Debit value is required!");
	  return;
	}

	var params ={Descrription:debitdesc,Category:debitcategory,Credit:0,Debit:debitAmount};
	var result = IB_runProc("usp_update_trans",params);

	$("#debitcategory").val("(none)");
	$("#debitAmount").val("0.00");
	$("#debitdesc").val("");
	refreshTable();
   }

   function refreshTable(){
 	  var arr = IB_runProc("usp_get_trans_data",{});

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

	  /* --------------------------------------------------------------*/ 
	  $( document ).ready(function() {
		refreshTable();
	  });
</script>
  
' 
 SELECT html = [ibadi].[html](@html)  
END

```

The main things to notice are:

**1. The use of W3.CSS framework** for all the styling. W3.CSS is responsive i.e. the components of your page automatically adjust, rearrange and rescale depending on weather you’re using a laptop, tablet or mobile phone. As a result of the use of W3.CSS you will find that the object tags are assigned with "w3-…" classes. e.g.
  ```
  <button class="w3-button w3-black w3-right" onclick="addCredit()">Add</button>
  ```
Find out more about W3.CSS at   https://www.w3schools.com/w3css/default.asp


**2. The use of jQuery** to find objects on the page in order to populate them with data. E.g. the balance at the top of the page is found and the value is set by saying something like:                     
  ```
  $("#PlaceholderForCurrentBalance").text(…);
  ```
You can learn more about jQuery at 
https://www.w3schools.com/jquery/default.asp

**3. The use of the IB_runProc function** to run stored procedures by passing JSON format parameter values and receiving an object result that can be resolved into arrays. 

This is used to populate tables, textboxes etc on the page. IB_runProc is also used for actions such as saving, updating or deleting.
 
e.g. to save data to our transaction table
```
var _descrription ="Money from Dad"
    _category="Gift"
    _credit=50.00
    _debit=0
	
var params ={Descrription:_descrription,Category:_category,Credit:_credit,Debit:_debit};
var result = IB_runProc("usp_update_trans",params);
```

The result will be `[{result: "OK"}]`

The final product looks like this on a PC

![credit_tracking_tool_on_PC](https://github.com/SamuelAina/IBADI-DOCS/blob/master/images/credit_tracking_tool_on_PC.png?raw=true)


The final product looks like this on the mobile phone.

![credit_tracking_tool_on_mobile](https://github.com/SamuelAina/IBADI-DOCS/blob/master/images/credit_tracking_tool_on_mobile.png?raw=true)


