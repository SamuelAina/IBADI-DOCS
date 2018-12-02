---
title: Homex
---
## Welcome to IBADI

Imagine if you could develop an entire web site from within your SSMS (SQL Server Management Studio). No HTML files, no CSS files, no JavaScript files. No ASP, no C# - Just write a bunch of stored procedures and your website is ready. This is the whole idea behind IBADI - **I**ntegrated **BA**ckend **D**evelopment **I**nterface.

Let's just dive in with the classic "hello world" demo.

```sql
CREATE PROCEDURE [webpage].[helloWorldIBADI]
AS
BEGIN
 DECLARE @html varchar(max)  
 SELECT  @html=  
'  
<div class="stamp">
<div class="greet">
   <p>Hello World!</p>
   <p>Welcome to IBADI!</p>
</div>
</div>

<style>
/* Using great stamp styling idea found at https://codepen.io/orhanveli/pen/tbGJL */
*{margin: 0; padding: 0;}
body {
    background: black;
    padding: 100px;
    text-align: center;
}

.stamp {
    width: 280px;
    height: 180px;
    display: inline-block;
    padding: 10px;
    background: white;
    position: relative;
    -webkit-filter: drop-shadow(0px 0px 10px rgba(0,0,0,0.5));
    /*The stamp cutout will be created using crisp radial gradients*/
    background: radial-gradient(
        transparent 0px, 
        transparent 4px, 
        white 4px,
        white
    );

    /*reducing the gradient size*/
    background-size: 20px 20px;
    /*Offset to move the holes to the edge*/
    background-position: -10px -10px;
}
.stamp:after {
    content: "";
    position: absolute;
    /*We can shrink the pseudo element here to hide the shadow edges*/
    left: 5px; top: 5px; right: 5px; bottom: 5px;
    /*box-shadow: 0 0 20px 1px rgba(0, 0, 0, 0.5);*/
    /*pushing it back*/
    z-index: -1;
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
.greet {
    background: white;
	padding:25%;
}
</style>
'
 SELECT html = [ibadi].[html](@html)     
END


```

Once this stored procedure is compiled in your SSMS use the following URL to view the result.

`http://localhost:99999/index.html?page_name=helloWorldIBADI` 

Which gives you:

![Image of simple example](https://github.com/SamuelAina/IBADI/blob/master/images/example_ibadi_2.png?raw=true)


Despite its simplicity, you can write some astonishingly sophisticated web pages with IBADI. It can accommodate all HTML tags including `<script>` and `<style>`. It can also be used with existing frameworks such as jQuery, AngularJS, Kendo UI, Bootstrap, W3CSS etc.

Before you can begin using IBADI with your SQL Server you need to set up your IBADI database and your IBADI website. Fortunately, all the code you need is available in **xxx_to_be_specified_xxx** on GitHub. Go through the following steps to setup these components.

docs_list_title: ACME Documentation
docs:

- title: Introduction
  url: introduction.html

- title: Configuration
  url: configuration.html

- title: Deployment
  url: deployment.html
