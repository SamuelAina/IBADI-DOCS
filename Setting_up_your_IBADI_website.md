---
title: Setting up your IBADI website
---
## Setting up your IBADI website 

There are various options for setting up your IBADI website:

Option 1 - You could use MS Visual Studio.
Option 2 – You could set up your IBADI website directly in IIS.

There are many other options of installing an IBADI server such as adding the folder to an existing website or installing IBADI on azure but I will leave these alone for now and focus on the two above.

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