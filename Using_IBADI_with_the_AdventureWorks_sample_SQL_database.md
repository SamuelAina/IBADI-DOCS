## Using IBADI with the AdventureWorks sample SQL database

AdventureWorks is a fictitous Bicycle rental company used for the purpose of demonstration therefore we will be using it to demonstrate some of the things you can achieve with IBADI. A full scale database is available for download at https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017

Download a copy for the right version of your SQL Server and follow the provided instructions for setup and instalation.

Once you have setup your AdventureWorks database you are ready to run the SQL script that will activate the database for IBADI.

Run the fallowing SQL script in MSMS

```
USE AdventureWorks
GO

CREATE SCHEMA webpage
GO

CREATE SCHEMA ibadi
GO

create proc [ibadi].[usp_check_if_proc_exists]  
 @procName  varchar(255)  
AS  
BEGIN  
  if exists(
  SELECT * 
  FROM INFORMATION_SCHEMA.ROUTINES 
  where  ROUTINE_SCHEMA = 'webpage' 
  and    specific_name=@procName
  )  
  BEGIN  
    SELECT result='1'  
  END  
  ELSE  
  BEGIN  
    SELECT result='0'  
  END  
END
GO

CREATE FUNCTION [ibadi].[html]
(
	@html varchar(max)
)
RETURNS varchar(max)
AS
BEGIN
	  SELECT  @html='[{"html":"'+replace(@html,'"','\"')+'"}]'

	  SELECT   @html = REPLACE(@html,CHAR(10)+CHAR(13),' ')  
	  SELECT   @html = REPLACE(@html,CHAR(13)+CHAR(10),' ')  
	  SELECT   @html = REPLACE(@html,CHAR(10),' ')  
	  SELECT   @html = REPLACE(@html,CHAR(13),' ')  
	  SELECT   @html = REPLACE(@html,CHAR(9),' ')   	

      RETURN  @html  
END
GO
```

To use your Adventureworks database with the IBADI website, you need to add the connection string to the web.config file.

```XML
<?xml version="1.0"?>
<configuration>
  <appSettings/>
  <connectionStrings>
    <!-- REMEMBER TO CHANGE THIS TO POINT TO YOUR OWN DATABASE!!!-->
    <add name="WbConnectionString" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=YOUR_DRIVE:\YOUR-FOLDER\AdventureWorks.mdf;Integrated Security=True" providerName="System.Data.SqlClient"/>   
  </connectionStrings>
  <system.web>
    <compilation debug="true" targetFramework="4.0">
      <assemblies>
        <add assembly="System.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/>
        <add assembly="System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/>
        <add assembly="System.Net, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/>
      </assemblies>
    </compilation>
    <authentication mode="Windows"/>
    <pages controlRenderingCompatibilityVersion="3.5" clientIDMode="AutoID"/>
  </system.web>
</configuration>
```

Now we can test the setup by compiling a webpage stored procedure and viewing it.

```Sql
CREATE PROCEDURE [webpage].[helloWorldIBADI]
AS
BEGIN
 DECLARE @html varchar(max)  
 SELECT  @html=  
'  
<div class="stamp">
<div class="greet">
   <p>Hello World!</p>
   <p>Welcome to IBADI in AdventureWorks!</p>
</div>
</div>
'
 SELECT html = [ibadi].[html](@html)     
END
```

To view the oage go to the following URL:
```http://localhost:60382/index.html?page_name=helloWorldIBADI```

**Remember to replace "localhost:60382" with your own host name (and port number if required)**

Here is the result. Not spectacular but it shows that it works. So we can go on to do some more impressive stuff in AdventureWorks.

![Welcome_to_IBADI_in_AdventureWorks](https://github.com/SamuelAina/IBADI-DOCS/blob/master/images/Welcome_to_IBADI_in_AdventureWorks.PNG?raw=true)




 
