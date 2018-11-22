# Errors

Main error codes returned:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Invalid request, wrong parameters or invalid values specified.
401 | Unauthorized -- STATIC_AUTH_TOKEN or JWT wrong or expired.
404 | Not Found -- Endpoint or resource not found or unsupported.
415 | Unsupported Media Type -- Wrong type of accepted request.
500 | Internal Server Error -- Error during entity creation or retrieval; check the log files.
