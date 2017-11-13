Important Notes:
Due to limitations in servicenow Application Scope functionality, we need to make some manual adjustments for app to function smoothly.

Ensure Global application scope
===============================
1. Ensure that your are in global application scope 
2. Click on gear icon on top right corner
3. this will open system settings popup
4. Click on devloper tab
5. Seleciton Global in Application drop down


Update script includes
======================
Following script includes reside in global scope and their accessibility must be updated to allow access from application scope.
Script includes:
	-JavascriptProbe
	-XMLDocument
Steps to make change:
1. Go to list of script include
2. Search for script include name
3. Update Accessible from field value to "All Application Scopes"

Update REST connection timeout properties
=========================================
This application uses REST APIs to transfer files. When files are larger it can take more time to transfer files. To ensure connection is not terminated when file is in transit, timeout properties should be updated.

1. Go to System Definition > Transaction Quota Rules
2. Filter results "Name Starts with REST"
3. Update "REST Attachment API request timeout" property to 299 seconds for following properties


Create Script action
=====================
ServiceNow does not support GlideImportSetLoader and GlideImportSetTransformer classes in application scope. For file transfer from mid server these classes are need. So we will need to create scope file in global scope

Steps to make change:
1. Go to script actions and search for "MSFI_Process Data Source"
2. right click on head and select "insert and stay". This will create copy of script action in global scope
3. Change file name to "MSFI_Process Data Source Global" (just to distinguish between two script actions)
4. Select the checkbox in active field
5. Save the record.

Update the mid server name in properties:
=========================================
1. Identify the mid server you would like to use for integration and verify server is up and running.
2. Restart the identified mid server (Do not skip this step)
3. Go to Left navigation menu >Mid Server File Transfer > Properties
4. Update the Mid Server Name property with Mid server name





How it works: (Data Load)
=============

Define file configurations.

1. Execute Job with file configuration info
2. JavaScript Probe sent to mid server
3. Search for the file(s) based on criteria, if found process else return
4. Transfer file(s) to data source or record selected
5. Process the file
	- Load the file 
	- Create Import Set
	- Process Transform Map
	- Data Loaded

