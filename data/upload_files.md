# How to persist files sent to my API via http?

- Any script can retrieve the parameters it receives using the native **request** object that allows you to retrieve information about the request, including the conveyed parameters
- Upload files can be retrieved using the **files** property of the **request** object
- To persist data into document from within a script, you need to require the native **document** module

## Getting the uploaded files

**request.files** is an map of key/values where each key is the name of the parameter used to upload files and the value is an array of File objects

Open your [workspace](https://www.scriptr.io/workspace) and create a new script with the below content
```
var files = request.files;
if (request.files.camera_snapshot && request.files.camera_snapshot.length > 0) {
    var snapshotFile = request.files.camera_snapshot[0];
    return snapshotFile;
};

return null;
```
 
Let's upload a file to the above API using [Postman](https://www.getpostman.com/)

![Upload file](./images/get_files.png)

*Image 1*

Issuing this request results in the below. As you can see, our API returned an array of File objects containing one element, as expected.

![Upload result](./images/upload_file_return.png)

*Image 2*

## Saving the file into a document

- Scriptr provides you with a NoSQL database that allows you to save data into key/value structures called "documents"
- To manipulate documents, you need to require the **document** module ([read more about creating documents](./persist_data.md))
- To create a document, you need to invoke the **document.create()**, passing the data to persist and any required descriptive metadata
- Files are attached into scriptr documents that are persisted in the No SQL storage

Let's update the above code example, to save the file into a document

```
var document = require("document");
var files = request.files;
if (request.files.camera_snapshot && request.files.camera_snapshot.length > 0) {
    
    var snapshotFile = request.files.camera_snapshot[0];
    var data = {
    	"attachments": snapshotFile, 
        "meta.types": {
            "attachments": "file"
        }
    };
    
    var resp = document.create(data);
    return resp;
};

return null;
```
To create the document, we need to following:
- Create a data structure with at least two properties: "attachments" and "meta.types"
- Associate the  uploaded file to "attachements" 
- Informing script.io that this field is of type "file" using the "meta.types" property
- Invoke document.create() passing the whole data structure

**ATTENTION**: **attachments** is a reserved keyword in scriptr. It is mandatory to use it to name a document field that contains a file, when the document is not bound to a **schema** (as in our example). In that latter case, you cannot use any field name but **attachments**. 

If you issue again a request towards our API using Postman, you should obtain something close to the below

![Save successful](./images/save_file_return.png)

*Image 3*

## Saving the file into a document that is bound to a schema

Schemas are used to define document types, i.e. constraints on the data (mandatory fields, multiplicity, data types, etc.). (Learn more about [schemas](./create_schema.md))

- Assume we want to save an uploaded camera snapshot file into a document 
- Also assume that we want the field to hold the document to be named "camera_snapshot"
- In that case we need to bind our document to a schema similar to the following (let's say we named this schema "surveillance_cam_doctype"):

```
<schema>
	<aclGroups>
		<aclGroup name='authenticated'>
			<read>authenticated</read>
			<write>authenticated</write>
			<fields>
				<field>camera_snapshot</field>
			</fields>
		</aclGroup>
		<schemaAcl>
			<read>nobody</read>
			<write>nobody</write>
			<delete>nobody</delete>
		</schemaAcl>
	</aclGroups>
	<fields>
		<field name='camera_snapshot' type='file'>
			<validation>
				<cardinality min='1' max='1'/>
			</validation>
		</field>
	</fields>
</schema>
```

All we need is to update our code once again as follows:
- Replace "attachments" with any field name we need ("camera_snapshot")
- Replace "meta.types" with "meta.schema" and refer to the schema we are binding our document to

```
var document = require("document");
var files = request.files;
if (request.files.camera_snapshot && request.files.camera_snapshot.length > 0) {
    
    var snapshotFile = request.files.camera_snapshot[0];
    var data = {
    	"camera_snapshot": snapshotFile, 
        "meta.schema": "surveillance_cam_doctype"
    };
    
	var resp = document.create(data);
    return resp;
};

return null;
```


