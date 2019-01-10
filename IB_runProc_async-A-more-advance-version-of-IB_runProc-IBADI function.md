##IB_runProc_async - A more advance version of IB_runProc IBADI function

There is a downside to running IB_runProc. Everything still works okay but it is not optimal due to the way browsers render content. In majority of the casses, for simple web applications IB_runProc is okay. If you open the console in the developer tool in your browser (F12 for google Chrome and select the console tab) you may find the following warning 
```
"jquery.min.js:4 [Deprecation] Synchronous XMLHttpRequest on the main thread is deprecated 
because of its detrimental effects to the end user's experience. For more help, check https://xhr.spec.whatwg.org/."
```
If you want to avoid this warning then you may use IB_runProc_async instead of IB_runProc. But you can't just substitute one for the other in your code. For IB_runProc you can do something like: 
```rslt = IB_runProc("procname",{})``` 
This means your browser will wait for IB_runProc to finish running and then put the result inside rslt and you can proceed to used it in your javascript code. When you use IB_runProc_async, it does not wait and you cannot simply capture rslt. You will have to do it differently. You will have to proccess anything that uses the result withing a callback function like this:
```
    IB_runProc_async(
                "procname"
                ,{}
                ,function (rslt) {
                    /*Do your processing here with rslt*/	
                }
    ); 
```	
	
Let us rewrite the "Retrieve a single value with data from IB_runProc" in this article <a href='https://samuelaina.github.io/IBADI-DOCS/Accessing_Data_with_IBADI_IB_runProc_function.html'>Accessing Data with IBADI IB_runProc function</a>

It used to be:
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

With IB_runProc_async it should become:

```
	CREATE PROC webpage.[Get Total Employee Count Async]
	AS
	BEGIN
	  DECLARE @html varchar(max)=
	  '
		<h3>Get Total Employee Count</h3>
		<div id="dvGetTotalEmployeeCount"/>
		<script>
		   IB_runProc_async(
				"ibadi.usp_getTotalEmployeeCount"
			   ,{}
			   ,function (result) {
			   $("#dvGetTotalEmployeeCount").append("<span>Total employee count = "+result[0][0].TotalEmployeeCount+"</span>");
			  }
		   ); 	   
		</script>
	  '
	  SELECT html=[ibadi].[html](@html)  
	END
```

