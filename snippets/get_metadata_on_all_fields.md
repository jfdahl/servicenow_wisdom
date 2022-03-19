# Get the required metadata on all fields available to a given table.

During a discussion in the SNDEVS Slack Workspace, Danny posed the question:
```
Is there an OOB function that will return all the field labels and field names of a table?  I’m thinking like getTableFields(‘incident’) or something like that?  Does something like that exist?  Or would I need to create my own script include for that?
```

There is a challenge with getting field data for any table that extends another table. The inherited field metadata are not accessible on the extended tables, but only on the table on which they are initially created. It becomes necessary to traverse the table ancestry to the base table and collection the field data along the way. The snippet below will start at the given table and walk up to the base table as defined in the `sys_db_object` table. At each table, the fields defined in the fieldList will be collected from the `sys_dictionary` table and added to the collection. Finally, the collection will be returned as a JavaScript object.


```javascript
function getFields( tableName, fields ){
  var fields = fields || [];
  var fieldList = [ 'element', 'column_label', 'internal_type', 'reference' ];
  
  var parent = new GlideQuery( 'sys_db_object' )
  .where( 'name', tableName )
  .select( 'super_class.name' )
  .toArray( 1 )[0].super_class;
  
  new GlideQuery( 'sys_dictionary' )
  .where( 'name', tableName )
  .where( 'internal_type', '!=', 'collection' )
  .select( fieldList )
  .forEach( function( field ){ fields.push( field ) } );
  
  if( parent ){ 
    fields = getFields( parent.name, fields ); 
  }
​
  return fields;
}
```

Running the snippet above against the Incident table will produce the following output:

```javascript
[
  {
    "element": "business_stc",
    "column_label": "Business resolve time",
    "internal_type": "integer",
    "reference": null,
    "sys_id": "2d55fa458d272010d152b9319a2024a2"
  },
  {
    "element": "calendar_stc",
    "column_label": "Resolve time",
    "internal_type": "integer",
    "reference": null,
    "sys_id": "ad55fa458d272010d152b9319a2024a1"
  },
  {
    "element": "caller_id",
    "column_label": "Caller",
    "internal_type": "reference",
    "reference": "sys_user",
    "sys_id": "a955fa458d272010d152b9319a20249c"
  },
  {
    "element": "category",
    "column_label": "Category",
    "internal_type": "string",
    "reference": null,
    "sys_id": "2555fa458d272010d152b9319a20249f"
  },
  {
    "element": "caused_by",
    "column_label": "Caused by Change",
    "internal_type": "reference",
    "reference": "change_request",
    "sys_id": "2d55fa458d272010d152b9319a20249b"
  },
  {
    "element": "child_incidents",
    "column_label": "Child Incidents",
    "internal_type": "integer",
    "reference": null,
    "sys_id": "ff97b2098d672010d152b9319a202448"
  },
  {
    "element": "close_code",
    "column_label": "Close code",
    "internal_type": "string",
    "reference": null,
    "sys_id": "5867b6058d672010d152b9319a202401"
  },
  {
    "element": "hold_reason",
    "column_label": "On hold reason",
    "internal_type": "integer",
    "reference": null,
    "sys_id": "1d88fa4d8d672010d152b9319a20243e"
  },
  {
    "element": "incident_state",
    "column_label": "Incident state",
    "internal_type": "integer",
    "reference": null,
    "sys_id": "a555fa458d272010d152b9319a20249e"
  },
  {
    "element": "notify",
    "column_label": "Notify",
    "internal_type": "integer",
    "reference": null,
    "sys_id": "2955fa458d272010d152b9319a20249d"
  },
  {
    "element": "parent_incident",
    "column_label": "Parent Incident",
    "internal_type": "reference",
    "reference": "incident",
    "sys_id": "fb97b2098d672010d152b9319a202452"
  },
  {
    "element": "problem_id",
    "column_label": "Problem",
    "internal_type": "reference",
    "reference": "problem",
    "sys_id": "e555fa458d272010d152b9319a202499"
  },
  {
    "element": "reopened_by",
    "column_label": "Last reopened by",
    "internal_type": "reference",
    "reference": "sys_user",
    "sys_id": "2955fa458d272010d152b9319a2024a4"
  },
  {
    "element": "reopened_time",
    "column_label": "Last reopened at",
    "internal_type": "glide_date_time",
    "reference": null,
    "sys_id": "a955fa458d272010d152b9319a2024a3"
  },
  {
    "element": "reopen_count",
    "column_label": "Reopen count",
    "internal_type": "integer",
    "reference": null,
    "sys_id": "3f97b2098d672010d152b9319a20243f"
  },
  {
    "element": "resolved_at",
    "column_label": "Resolved",
    "internal_type": "glide_date_time",
    "reference": null,
    "sys_id": "1067b6058d672010d152b9319a202435"
  },
  {
    "element": "resolved_by",
    "column_label": "Resolved by",
    "internal_type": "reference",
    "reference": "sys_user",
    "sys_id": "5867b6058d672010d152b9319a202438"
  },
  {
    "element": "rfc",
    "column_label": "Change Request",
    "internal_type": "reference",
    "reference": "change_request",
    "sys_id": "ad55fa458d272010d152b9319a20249a"
  },
  {
    "element": "severity",
    "column_label": "Severity",
    "internal_type": "integer",
    "reference": null,
    "sys_id": "2155fa458d272010d152b9319a2024a1"
  },
  {
    "element": "subcategory",
    "column_label": "Subcategory",
    "internal_type": "string",
    "reference": null,
    "sys_id": "a155fa458d272010d152b9319a2024a0"
  },
  {
    "element": "sys_id",
    "column_label": "Sys ID",
    "internal_type": "GUID",
    "reference": null,
    "sys_id": "6555fa458d272010d152b9319a2024a5"
  }
]
```