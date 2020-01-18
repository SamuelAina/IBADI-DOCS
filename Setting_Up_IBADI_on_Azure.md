## Setting Up IBADI on Azure

Okay, this one could be a big one, as it could open up some exciting prospects. If you put your IBADI in Azure you’re laying down the tracks for rapid website application development readily available to the public.

So, here are the steps:


### Create your azure account

It wouldn’t do for me to outline here how this can be done when it is already well documented elsewhere. So follow the steps here: 
https://docs.microsoft.com/en-us/learn/modules/create-an-azure-account/


### Create an azure Web app

A Web App is where the pages can be accessed by the user. It is the place where the link/URL will take the users. The Web App can be considered on this implementation to be the same as the Web Site. 

Go through the steps below to create a Web App in Azure. Here I call it IBADI, but you can give yours any name that you want.

1.  Select the “New” menu and click on the Web App icon among the list of items that can be created.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_1.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_1"/>


2. Provide the necessary responses for the configuration settings requested in the dialog window.

Note that you may need to create a new resource group if you haven’t got one already. A resource in Azure is a general name for such things as Virtual machines, storage accounts, web apps, databases etc.

If you are creating a new resource group, the name will be required.
 
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_2.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_2"/>

Once you have specified the resource, go on and complete all the other required entries.
You will see from the screen shots below the values I have used in my own settings.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_3.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_3"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_4.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_4"/>

Review or create your App.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_5.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_5"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_6.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_6"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_7.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_7"/>

### Publish IBADI files in your Web App

Next you need to upload the IBADI web files to your Azure Web App. Download the Web folder from https://github.com/SamuelAina/IBADI to a location on your drive. 

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_8.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_8"/>


Click on the Get publish profile menu at the top of the Azure overview page for the newly created Web App and download the resulting file. It is an XML file and it contains details for accessing your Azure virtual folder for the WebApp.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_9.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_9"/>

There are many ways of getting your web files into the Azure web folder of your Web App, a process which is called “Publishing”. On this occasion I’m using the FTP option to publish the files that were downloaded from GitHub.

The FTP tool that I’m using is FileZilla and I used the credentials from the XML file I got from the “Publish Profile menu”.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_10.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_10"/>



### Create your SQL Server database

The engine room for your IBADI is the SQL Server database. So, if you do not have one you can create it within Azure.
 
Click on the Create resource menu on the Azure platform and select SQL Database:

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_11.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_11"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_12.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_12"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_13.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_13"/>

Use the same resource group that you used for the WebApp (e.g. IBADI-RS)

You may need to create a server if you haven’t created one already. This is the box (a virtual one, that is) on which your database will be installed, which has its own windows operating system. Provide general details of the server – Name, log-in etc.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_14.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_14"/>

Once you have provided a server, go ahead and add the SQL Server database details.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_15.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_15"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_16.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_16"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_17.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_17"/>

Be careful so that your options do not attract a hefty service charge.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_18.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_18"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_19.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_19"/>

In my case, I changed my selections in the configuration tab as shown below to reduce the cost from 325.98GBP/month (yikes!!) to 4.65GBP/month: 

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_20.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_20"/>

Finally, create your SQL database. I have called mine IBDB001 but you can call yours anything you want. Also, you need to create a database user access that your IBADI App can use to connect to the database.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_21.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_21"/>
<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_22.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_22"/>

One thing you should remember to do is to click on the “set sever firewall” so that you can set up your PC or laptop to access the database. 

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_23.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_23"/>

You will need to add the IP address of your PC/Laptop where your SSMS is installed – the place where you will be writing your SQL queries and creating your stored procedures etc.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_24.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_24"/>

### Edit the web.config file.

Once you have created your database, you need to retrieve the database connection details. 

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_25.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_25"/>

Amend the connection string in your web.config file and ftp it back to the web server.

### Set up IBADI SQL objects

Use SQL Server Management Studio or any suitable alternative to open the database

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_26.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_26"/>

Download the SQL configuration script from https://github.com/SamuelAina/IBADI/blob/master/IBADI-SETUP.SQL

Be sure to reconnect to the required database and run the script within the new database



### Finally, add a sample IBADI application to check that your setup is okay.

Use the Hello World application at this link - https://samuelaina.github.io/IBADI-DOCS/index.html. Copy the SQL, paste it into your query window and execute.

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_27.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_27"/>

Use the following URL to run your application:
https://ibadi.azurewebsites.net/index.html?page=helloWorldIBADI

And the result, if all has been correctly set up is:

<img src="https://github.com/SamuelAina/IBADI-DOCS/raw/master/images/Setting_Up_IBADI_on_Azure_img_28.PNG?raw=true%22" alt="Setting_Up_IBADI_on_Azure_img_28"/>

Now, the world is your lobster!!!


