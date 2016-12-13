# Errors

Pathover API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- The request contains missing/invalid parameters. The reason text will more detailed messages.
403 | Forbidden -- The API key is invalid, please check again.
404 | Not Found -- The requested resource doesn't exist or missing.
405 | Method Not Allowed -- The endpoint your are trying to use does not exist.
500 | Internal Server Error -- Something occurred in Pathover API. Please try again later.
