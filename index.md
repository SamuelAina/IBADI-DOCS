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

`http://localhost:99999/index.html?page_name=helloWorldIBADI` 

Which gives you:

![Image of simple example](https://github.com/SamuelAina/IBADI/blob/master/images/example_ibadi_1.png?raw=true)


Despite its simplicity, you can write some astonishingly sophisticated web pages with IBADI. It can accommodate all HTML tags including `<script>` and `<style>`. It can also be used with existing frameworks such as jQuery, AngularJS, Kendo UI, Bootstrap, W3CSS etc.

Before you can begin using IBADI with your SQL Server you need to set up your IBADI database and your IBADI website. Fortunately, all the code you need is available in **xxx_to_be_specified_xxx** on GitHub. Go through the following steps to setup these components.
