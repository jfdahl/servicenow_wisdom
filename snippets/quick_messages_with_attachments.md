# Quick Messages with attachments

Quick messages are a great way to pre-define messages that agents can send to 
users based on common scenarios. For example, a user can send a quick message 
to a custom with a standard guide based on the customer's need. Attachments 
added to Quick Messages are not copied to the email generated from them, but 
there is a way to make them available with a business rule.  

Create a BEFORE INSERT business rule on the `sys_email` table to add the attachments from the referenced quick message.  
- Condition: body contains: `{{{QMAttachment:quick_message_sys_id}}}`
- Actions:
  - Add attachments from qm to email
  - Remove `{{{QMAttachment:quick_message_sys_id}}}` text. 


`NOTE: Quick Message author will need to include {{{QMAttachment:attachment_sys_id}}} in the body of the message.`


```javascript
(function executeRule(current, previous /*null when async*/ ) {
    /*
     * Given an email created from a Quick Message with a pre-defined attachment tag,
     * copy the attachments from the Quick Message to the email and remove the attachment tag.
     */
    var qm_tag, qm_sys_id, gr_attachments;
    var attachment_tag_pattern = /\{\{\{QMAttachment:[a-fA-F0-9]{32}\}\}\}/g;

	// Get the sys_id of the Quick Message
    qm_tag = current.body.match( attachment_tag_pattern )
	
    if ( qm_tag ) {
        qm_sys_id = qm_tag[ 0 ].match( /[a-fA-F0-9]{32}/ )[ 0 ];
    } else {
        return;
    }

    // Get the attachments
    gr_attachments = new GlideRecord( 'sys_attachment' );
    gr_attachments.addQuery( 'table_sys_id', qm_sys_id );
    gr_attachments.query();

    // Copy any attachments from the Quick Message to the Email
    while( gr_attachments.next() ){
        try {
            GlideSysAttachment.copy( 'sys_email_canned_message', qm_sys_id, 'sys_email', current.getUniqueValue() );
        } catch (e) {
            gs.error('An error has occurred trying to copy Quick Message attachments to the email. \n' + e.name + ': ' + e.message);
        }
    }

    // Remove the attachment tag
    current.body = current.getValue( 'body' ).replace( attachment_tag_pattern, '' );



})(current, previous);
```