## Troubleshooting IBADI code
As you may have notced, the code in the IBADI page stored procedure is made out of sql varchar(MAX) variables. E.g.

```SQL
DECLARE @html varchar(max)  
SELECT  @html=  
'  
<div class="stamp">
<div class="greet">
   <p>Hello World!</p>
   <p>Welcome to IBADI!</p>
</div>
</div>
'
```

As a result of this, care must be taken with the sytax of the code, bearing in mind that the code may contain javascript, HTML, CSS and SQL.

### First off, mind the single quotes.
Since the varchar variables in SQL are started and ended with single quotes (e.g. ```SELECT @name='Joe Bloggs'```) if you are to use single quotes anywhere in the variable string you wo=ill have to double it up.  e.g. ```SELECT @name='This id Joe''s tea'```.

In HTML, CSS and JavaScript single quotes and double quotes are often interchangable, e.g. ```<DIV id="item1"></DIV>``` is the same as ```<DIV id='item1'></DIV>```. Where you can avoid it, you could just stick with double quotes throughout. If you still want to use single quotes then it would be something like ```<DIV id=''item1''></DIV>```

In any case, make sure that all quotes must be properlly balanced. E.g. the following is wrong:  ```<DIV id="item1></DIV>```

### All javascript lines must be terminated with a semicolon (;)

JavaScript within IBADI is not as tolerant with missing line terminations, so please **ALWAYS** remember to complete every line with ;. Not doing this will cause the page not to be properly displayed and it will not function properly. e.g:
```
DECLARE @script1 varchar(max)
SELECT  @script =
'
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
 </script> 
' 
```
  
### Don't  use line comments (e.g. ```//This is a comment```)  
Avoid using line comments //. Use block comments (/* ... */) instead.

Also be sure that all block comments are properly closed and note that nested block comments don't work.

### Ensure that all brackets are matched
This would apply to all brackets e.g. (), {}, [] etc which are used as part of function definitions, arrays, etc. This also  applies to CSS code. e.g.

```
DECLARE @style  VARCHAR(max)
SELECT  @style=  
'
<style>
*{margin: 0; padding: 0;}
body {
    background: black;
    padding: 100px;
    text-align: center;
}
/*Some text*/
.stamp:before {
    content: "CSS3";
    position: absolute;
    bottom: 0; left: 0;
    font: bold 24px arial;
    color: white;
    opacity: 0.75;
    line-height: 100%;
    padding: 20px;
}
</style>
'
```




