# Convert HTML to Text
When copying HTML data into a plain string field, the tags can make the content difficult to read. This code will remove the tags and preserve the general layout of the text.

```js
function convertHtml2Text( htmlInput ){
  return htmlInput.replace( /(\r|\n)/g, '' )			// Remove new line and carriage returns
    .replace( /<br\s?\/>/ig, '\n' )			          // Replace break tags with new line characters
    .replace( /<\/?(p|div)>/ig, '\n' )	          // Replace paragraph and div tags with new line characters
    .replace( /<[^>]+>/ig, '' );				          // Remove html tags
}

```
