# String Array Conversion

During a discussion in the SNDEVS Slack Workspace, Abbey posed the question:
```
How do I convert [bob, mary, sam, henry]  to ['bob','mary','sam','henry'] ?
```

Several people contributed bits and pieces, with Chris Helming putting the final pieces together.  

```javascript
// Given a single string containing an array structure
var str = "[o'henry, brown sugar, wilkes-booth, sam, henry]";

// Run it through this code
var arr = str
    .match( /[^\[\]]/g )
    .join( '' )
    .split( ',' )
    .map( String.trim );

// Get a proper array object
gs.info( arr ); 
// Outputs: ["o'henry", "brown sugar", "wilkes-booth", "sam", "henry"]

```

