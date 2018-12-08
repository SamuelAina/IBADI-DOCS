---
title: Welcome to IBADI
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

![Image of simple example](https://github.com/SamuelAina/IBADI-DOCS/blob/master/images/example_ibadi_2.png?raw=true&class="img-fluid")

Despite its simplicity, you can write some astonishingly sophisticated web pages with IBADI. It can accommodate all HTML tags including `<script>` and `<style>`. It can also be used with existing frameworks such as jQuery, AngularJS, Kendo UI, Bootstrap, W3CSS etc.

Before you can begin using IBADI with your SQL Server you need to set up your IBADI database and your IBADI website. Fortunately, all the code you need is available at https://github.com/SamuelAina/IBADI on GitHub. Go through the following steps to setup these components.

## Setting up your IBADI website 

There are various options for setting up your IBADI website:

Option 1 - You could use MS Visual Studio.
Option 2 – You could set up your IBADI website directly in IIS.

There are other other options of installing an IBADI server such as adding the folder to an existing website or installing IBADI on azure but I will leave these alone for now and focus on the two above.

### Option 1 - Using MS Visual Studio.
1. Download the Web folder from https://github.com/SamuelAina/IBADI to a location on your drive. Open the folder and click on the .SLN file so that it is opened visual studio.

2. Edit the connection string in the web.config file by putting the details of your IBADI database.

3. Once you have edited your web.config, run the website by clicking on the green arrow icon.

![Image of Web.config](https://github.com/SamuelAina/IBADI-DOCS/blob/master/images/IBADI_web_config.png?raw=true)

If all goes well, you should see a blank page and the address bar should contain something that looks like this: 

`http://localhost:57942/index.html` 

to display your webpage add the page name to the end of this link, e.g. 

`http://localhost:57942/index.html?page_name=helloWorldIBADI` 

and that’s it.

You can also use the Visual Studio to publish your IBADI Website to a server by right clicking on the solution and selecting the "Publish Web Site" option on the ensuing menu. Just make sure that edit the connectionString to point to a server database.

### Option 2 – Setting up your IBADI website directly in IIS.

The following instructions based on Microsoft Internet Information Services (IIS) version 7.0 running on Microsoft Windows Server 2008 have been taken from https://support.microsoft.com/en-gb/help/323972/how-to-set-up-your-first-iis-web-site

1.	Simply download the IBADI Web folder from xxherexx to a location on a drive on the web server.
2.	Click Start, point to Settings, and then click Control Panel.
3.	Double-click Administrative Tools, and then double-click Internet Services Manager.
4.	Click Action, point to New, and then click Web Site.
5.	After the Web Site Creation Wizard starts, click Next.
6.	Type a description for the Web site. 

This description is used internally to identify the Web site in Internet Services Manager only.
7.	Select the IP address to use for the site. 

If you select All (unassigned), the Web site is accessible on all interfaces and all configured IP addresses.
8.	Type the TCP port number to publish the site on.
9.	Type the Host Header name (the real name that is used to access this site).
10.	Click Next.
11.	Either type the path to the folder that is holding the Web site documents or click Browse to select the folder, and then click Next.
12.	Select the access permissions for the Web site, and then click Next.
13.	Click Finish.

Once you have set up your website the URL for your IBADI page will be in the following form:  
`http://www.yourwebname.com/index.html?page_id=yourwebpage`
There are other options of installing an IBADI server such as adding the folder to an existing website or installing IBADI on azure but I will leave these alone for now.
Anyway, once you have completed setting up your IBADI website and your IBADI database you can proceed with building your awesome web pages. I will be posting examples on this platform.

## Setting up your IBADI database

1. Download the SQL script from https://github.com/SamuelAina/IBADI/blob/master/IBADI-SETUP.SQL
2. Go to your database server and open the file in SSMS.
3. Optionally you may change the name of the database in the script from ibadidb to whatever you want.
4. Run the SQL script within your SSMS while in the master database.

 



