# How to express constraints on my data (schemas)?

- Scriptr.io provides you with a NoSQL database that allows you to save data into key/value structures called "documents"
- You can create (XML) schemas to define constraints on the data (mandatory fields, multiplicity, data types, etc.)

## Create a schema using the visual environment

From the scriptr.io [workspace](https://www.scriptr.io/workspace) click on **Tools** in the toolbar then select **Data Explorer**

![Open Data Explorer](./images/open_data_explorer.png)

*Image 1*

Click on Schema then New to open the schema editor.

![Open Schema Editor](./images/new_schema.png)

A schema has two main sections: aclGroups and fields.

### aclGroups 

ACL groups allow you to define read/write permissions on the fields of your document. You should define at least one ACL group. 
Scriptr.io creates three ACL groups by defaults: 
- &lt;aclGroup name='aclgroupName'&gt; (the name is just a dummy value, you should replace it with somethings meaningful), 
- &lt;defaultAcl&gt; is the default ACL. You can remove this element if you wish
- &lt;schemaAcl&gt; defines permissions on the schema itself. **You usually won't modify it**

An ACL group specifies read/write permissions on the document fields it contains (&lt;fields&gt;)
Permissions are specified via the **&lt;read&gt;** and **&lt;write&gt;** elements, which respectively define who has read and/or write permissions on the fields.

For example
```

```


