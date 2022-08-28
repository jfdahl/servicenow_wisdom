A user who can view a list or table in ServiceNow has limited options for exporting data. Full admin rights are required to use the platform UI Actions to export data to XML files. The URL format below will allow any user with access to a table to extract the data contents to XML:

`https://<INSTANCE>.service-now.com/<TABLE_NAME>_list.do?XML&useUnloadFormat=true`



```javascript
var TABLE = 'incident',
    FORMAT = 'CSV';

var url = gs.getProperty("glide.servlet.uri") + TABLE + '_list.do?' + FORMAT;
if( FORMAT == 'XML' ){
    url +=  '&userUnloadFormat=true';
}
```



Other variables are also allowed:

- EXCEL
- CSV
- JSON
- sysparm_query=<ENCODED_QUERY>
- sysparm_force_view=<VIEW_NAME>  