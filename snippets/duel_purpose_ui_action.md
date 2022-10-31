A UI Action executes the script logic on the server. If the `Client` checkbox is checked, then any function referenced in the `onclick` field will be executed on the client before the server-side logic is executed. 
- Data cannot be passed directly from the client script to the server script (although GlideAjax calls can be run from the client).
- The `doClientAction` function name must match the UI Action's `onClick` field value.
- The third parameter in the `gsftSubmit` function call must match the `Action name` value.


```javascript
function doClientAction() {

    // Do things in the client

    // Call server side logic
    gsftSubmit(null, g_form.getFormElement(), 'action_name');
}

if (typeof window == 'undefined')

    // Do things in the server
    action.setRedirectURL(current);
```

