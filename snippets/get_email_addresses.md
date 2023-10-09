A requirement came up to:
- gather all of the "valid" email addresses from an unstructured set of text data,
- remove all duplicate entries,
- format the resulting values as semicolon delimited so it could be pasted into the Outlook TO field.

```javascript
function get_email_addresses( input_string ) {
    /*
    Given a string containing one or more email addresses,
    extract the valid email addresses, change them to lower case, sort them,
    remove duplicates, join them by semicolons, then return the string.
    */
    
    var email_address_regex = /[a-z0-9.\-_]+@[a-z0-9.-]+\.[a-z]{2,}/g,
	delimiter = '; ',
        email_address_list = input_string
            .toString()
            .toLowerCase()
            .match( email_address_regex )
            .sort()
            .filter( function( value, index, self ){
            	return self.indexOf( value ) === index;
            } );

    if ( email_address_list.length == 1 ) {

	    return email_address_list[ 0 ];

    } 

    return email_address_list.join( delimiter );

}
```
