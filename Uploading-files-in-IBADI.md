## Uploading files with IBADI

File uploading is something you may often want to do on a web page. A very common use case is uploading image files for product catalogues or photos into a photo galery. In this demo, I will use the IB_doFileUpload function to construct a page where you upload a file and then display it on the page.

This tutorial assumes that you have set up your IBADI database as well as your IBADI Web folder.

### OK, Here is the proc:
---
	CREATE PROCEDURE [webpage].[uploadexample]
	AS
	BEGIN
	 DECLARE @html varchar(max)  
	 SELECT  @html=  
	'  
	<div>
	 <form id=form1 runat=server role=form enctype="multipart\/form-data">  
	 <p>Choose file and click on OK to upload it</p>  
	 <div id="progress_container" style="visibility:hidden">
	   <font color=red>please wait...</font>
	   <progress id="upload_progress"   value="0" max="100"></progress>
	 </div>  
	 <input  type="file" id="file_upload" name="file" accept=".jpg" class="k-button" style="width: 680px;"/>  
	 </form>  
	 <button  style="margin-left: 300px;width:50px;"  onclick="uploadOKclicked()">OK</button> 
	 <p><img id="custom_image" alt="custom image"  width="400" height="400"></p>
	</div>

	<script>

	function  uploadOKclicked()
	 {
	   IB_doFileUpload("file_upload",
				"upload_progress", 
				"progress_container",
				function(uploadedFileURL){
				var target_image_container_element = document.getElementById("custom_image");
				target_image_container_element.src=uploadedFileURL;      
			}
		 );
	 }
	</script>
	'
	 SELECT html = [ibadi].[html](@html)     
	END
---

#### Things to note:

1. We included a `<form>` tag. This is required in order for the page to post information back to the server

2. We have a button `<button>` which when clicked calls a function - uploadOKclicked()

3. We have a `<div>` container to hold the status information and a progress bar (which is just a `<process>` tag)

4. We have an input tag of the type "file", this appears on the page as a button for selecting the file to be uploaded

5. Finally, note that uploadOKclicked() makes a call to  the IB_doFileUpload function which does all the upload process 


#### Let me say a bit more about IB_doFileUpload...

IB_doFileUpload requires four parameters:
1. file_upload_input_tag_id - which is simply the id of the file upload `<input>` tag (i.e. file_upload)

2. upload_progress_tag_id - this is the id for the <progress> tag (i.e. upload_progress). This is required so that IB_doFileUpload can continously upload the process while the file is uploading. 

3. progress_container_id  - this is the id for the progress container `<div>` (i.e. progress_container)

4. callback function - this function is where you specify what to do with the file once it has finished uploading. In this case we are using it to set the source of an `<img>` tag, so that the image can be displayed.



