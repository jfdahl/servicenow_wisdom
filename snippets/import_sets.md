# System Properties

Enable ServiceNow to automatically adjust the string length on the Import Set table when a new value is larger than the previously used values:
`com.glide.loader.verify_target_field_size = true`

To prevent a child import from starting if a parent has an error:
`glide.scheduled_import.stop_on_error = true`

Change the number of rows imported during Import Set Test Load (DOES NOT APPLY TO JSON OR XLSX):
`com.glide.loader.max_scan_rows = 50` // Defaults to 20

Concurrent Import Set Limit:
`glide.scheduled_import.max.concurrent.import_sets = 10`

Ignore non-parseable lines in CSV files:
`com.glide.csv.loader.ignore_non_parseable_lines = true`
