# Array Utils


### Product
```javascript
if (Array.prototype.product === undefined) {
    Array.prototype.product = function(){
        // Discard any values that are not numeric,
        // Multiply the remaining numeric values,
        // Return the total.

        return this.find( /\d/ ).reduce( function( total, addend ){
            return Number( total ) * Number( addend );
        })
    }
}
```

### Sum
```javascript
if (Array.prototype.sum === undefined) {
    Array.prototype.sum = function(){
        // Discard any values that are not numeric,
        // Sum the remaining numeric values,
        // Return the total.

        return this.find( /\d/ ).reduce( function( total, addend ){
            return Number( total ) + Number( addend );
        })
    }
}
```

### Multiply
```javascript
if ( Array.prototype.multiply === undefined ) {
    Array.prototype.multiply = function( num ){
        // Given a number,
        // Discard any non-numeric elements,
        // Multipe each numeric element by the number,
        // Return the result.

        if( typeof num !== 'number' ){ return this; }

        return this.find( /\d/ ).map( function( item ){
            return Number( item ) * num;
        })
    }
}
```

### Add
```javascript
if (Array.prototype.add === undefined) {
    Array.prototype.add = function( num ){
        // Given a number,
        // Discard any non-numeric elements,
        // Add each numeric element to the number,
        // Return the result.

        if( typeof num !== 'number' ){ return this; }

        return this.find( /\d/ ).map( function( item ){
            return Number( item ) + num;
        })
    }
}
```

### Max
```javascript
if (Array.prototype.max === undefined) {
    Array.prototype.max = function() {
        return Math.max.apply(null, this);
    };
}
```

### Min
```javascript
if (Array.prototype.min === undefined) {
    Array.prototype.min = function() {
        return Math.min.apply(null, this);
    };
}
```

### Find
```javascript
if ( Array.prototype.find === undefined ) {
    Array.prototype.find = function( criteria ){
       if( criteria instanceof RegExp ){
            return this.filter( function( item, idx, arr ){
                return item.toString().match( criteria );
            })
        }
        else{
            return this.filter( function ( item ){
                return item === criteria;
            })
        }
    }
}
```

### Unique

The array utilities in ServiceNow were created many versions ago and have not been updated as the JavaScript standard has matured. The code below will add a new method `.unique()` to the Array prototype. This version uses build-in function calls and runs significantly faster than the looping version in the ServiceNow array utilities script include. The only functional difference is that this version retains the FIRST instance of duplications in an array while the script include retains the LAST instance found.  
```javascript
if( Array.prototype.unique === undefined ){
    Array.prototype.unique = function(){
        return this.filter( function( item, idx, arr ){
            return arr.indexOf( item ) == idx;
        });
    }
}
```