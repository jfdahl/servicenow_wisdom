# Bookmarklets


## Introduction

For those who are not aware, bookmarklets are just normal browser bookmarks; however, instead of pointing to a website, the URL field contains JavaScript. When you have a page open and click the bookmark, the script executes `within the context of the current page`. This provides some interesting capabilities and should also be a clear reminder to always do server-side validation!  

The reason I started to collect bookmarklets is because I was working on multiple customer instances and I got tired of wasting time setting up my favorites across the various instances and forgetting which instances had "that one" that I needed. The main benefits I realize from using bookmarklets include:
- They are instance independent. They are always available in my browser, even after a clone or when I start working on a new instance. This could be a new customer, a new instance for my organization, or a new PDI.
- They can be setup to open in a new tab or the current tab.
- They can be setup to open with the navigation pane or in their own window.
- They can change the state of the current window directly, potentially bypassing a long or complex process. This is handy for unit testing and demos; however, `it can also allow users to bypass some client-side restrictions. Make sure you have server-side validation and restrictions in place.` Specifically, you can:
    - Change the values of fields, including if they are read-only.
    - Make mandatory fields not mandatory.
    - Save the record when the Save UI Action is not available.
    - Setup a sequence of field changes to walk through a demo without having to manually type or click each field. This means you won't forget a step and fail the demo.

When you create the bookmark, you can set any name that makes sense. When you set the URL field, make sure you include the `javascript:` part.

## Navigation
---

### Open Studio in a new tab
```javascript
javascript:window.open( '$studio.do?sysparm_transaction_scope=global&sysparm_nostack=true', '_blank');
```

### Open Scripts Background in the current tab
```javascript
javascript:window.open( 'sys.scripts.do' );
```

### Open the Plugin list
```javascript
javascript:window.open( "$allappsmgmt.do", "_blank" );
```

### Open a service portal (Prompt for portal name or default to `sp`)
```javascript
javascript:window.open( prompt('What portal URL do you want to open?') || 'sp', '_blank' );
```

### Open the system logs filtered to the last 15 minutes
```javascript
javascript:top.open( 'syslog_list.do?sysparm_query=sys_created_onONLast 15 minutes%40javascript%3Ags.beginningOfLast15Minutes()%40javascript%3Ags.endOfLast15Minutes()&sysparm_view=');
```
`Note: You can add any encoded query you want to filter the list... include your initials, set the source, etc.`

### Open Stats.do
```javascript
javascript:top.open( "stats.do");
```

### Open Xplore (Until it's fixed for Polaris)
```javascript
javascript:window.open( "snd_xplore.do", "_blank");
```

## Actions

### Cancel My Transactions
```javascript
javascript:top.open( "cancel_my_transaction.do");
```

### Clear Server Cache
```javascript
javascript:top.open( "cache.do");
```

### Create a new update set in the current scope
```javascript
javascript:window.open( 'sys_update_set.do?sys_id=-1', '_blank' );
```

### Make mandatory fields non-mandatory
```javascript
javascript: var f = window.frames['gsft_main'] !== undefined ? window.frames['gsft_main'].g_form : g_form; f.getEditableFields().forEach( field => f.setMandatory( field, false ) );
```

### Save the current record
```javascript
javascript: var f = window.frames['gsft_main'] !== undefined ? window.frames['gsft_main'].g_form : g_form; f.save();
```

### Set a field value: Prompt for field name and value
```javascript
javascript:var f = window.frames['gsft_main'] !== undefined ? window.frames['gsft_main'].g_form : g_form;f.setValue( prompt('Enter the field name:'), prompt('Enter the value:') );
```

### Set a field value: Short Description
```javascript
javascript:var f = window.frames['gsft_main'] !== undefined ? window.frames['gsft_main'].g_form : g_form; f.setValue( 'short_description', prompt( 'Enter the new value:' ) );
```

### Set User Preference: Row Count
```javascript
javascript:top.location += '&sysparm_userpref_rowcount=' + prompt('Set row count to: ');
```

### View Scratchpad - Alert
```javascript
javascript: var w = window.frames['gsft_main'] !== undefined ? window.frames['gsft_main'] : window; w.g_scratchpad = w.g_scratchpad || {"scratchpad":"empty"}; w.alert( "Scratchpad: \n" + JSON.stringify( w.g_scratchpad).replaceAll(/,"/g, ",\n\"").replaceAll("{", "{\n").replaceAll("}", "\n}") );
```

### View Scratchpad - Info Message
```javascript
javascript: var w = window.frames['gsft_main'] !== undefined ? window.frames['gsft_main'] : window; w.g_form.addWarningMessage( "Scratchpad: <br/>" + JSON.stringify( w.g_scratchpad).replaceAll(/,"/g, ",<br/>\"").replaceAll("{", "{<br/>").replaceAll("}", "<br/>}") );
```
