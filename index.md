## Welcome to IBADI

Imagine if you could develop an entire web site from within your SSMS (SQL Server Management Studio). No HTML files, no CSS files, no JavaScript files. No ASP, no C# - Just write a bunch of stored procedures and your website is ready. This is the whole idea behind IBADI - Integrated BAckend Development Interface.

Let's just dive in with the classic "hello world" demo.

```sql
CREATE PROCEDURE [webpage].[helloWorldIBADI]
AS
BEGIN
 DECLARE @html varchar(max)  
 SELECT  @html=  
	'  
	<div style="background-color: yellow;">
	<P>Hello World!</P>
	</div>
	'
 SELECT html = [ibadi].[html](@html)     
END

```

Once this stored procedure is compiled in your SSMS use the following URL to view the result.
