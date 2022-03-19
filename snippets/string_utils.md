```javascript
if (String.prototype.format === undefined) {
    String.prototype.format = function (values) {
        /*
        Given an integer, single string, array of strings, or object of strings,
        replace the position markers ({}, {0}, {1}...) with string value(s).
        Examples:
            'this is {} number.'.format(1);                                 // returns "this is 1 number"
            'this is {} number.'.format(true);                              // returns "this is 1 number"
            'this is a {}.'.format("string");                               // returns "this is a string."
            'This {0} array of {1}s!'.format(['is an', 'string']);          // returns "This is a list of strings!"
            'This {k1} an object of {k2}s!'.format({k1:'is', k2:'string'}); // returns "This is an object of strings!"
        */
        var string = this.toString();
        var value_type = typeof values;
        var value_length = values.length;

        if( value_type === 'number' ){
            values = String( values );
            value_type = 'string';
        }

        if( ['string','boolean'].includes( value_type ) ){
            string = string.split( /{.?}/ ).join( values );
        } 

        else if( ( value_type === 'object' ) && ( values.length !== undefined ) ){
            values.forEach( function( val, idx, arr ){
                string = string.split( "{" + idx + "}" ).join( val );
            })
        } 
        
        else if( ( value_type === 'object' ) && ( values.length === undefined ) ){
            for( var k in values ){
                string = string.split( "{" + k + "}" ).join( values[k] );
            }
        }

        return string;
    };
}

if (String.prototype.pad === undefined) {
    String.prototype.pad = function (pad_char, result_length) {
        /*
        Given a string character and an integer length,
        pad the string to the requested length with the character.
        If the integer is negative, then pad-left the string.
        */
        var pad_length, padding, result,
            string = this.toString(),
            pad_char = pad_char.toString(),
            result_length = parseInt(result_length);

        if (Math.abs(result_length) <= this.length ){ return string; }

        pad_length = Math.abs(result_length) - this.length;
        padding = pad_char.repeat(Math.abs(pad_length));
        
        if ( result_length > 0 ) {
            result = string + padding;
        }else{
            result = padding + string;
        }
        return result;
    };
}
```