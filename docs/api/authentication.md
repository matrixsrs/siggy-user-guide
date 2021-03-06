## Authentication

siggy's api uses HMAC authentication on messages.

All requests must have an Authorization header in the form:

```
Authorization: siggy-HMAC-SHA256 Credential=KEYID:HASH
```
and a Date header either set by `x-siggy-date` or the http standard `Date`. This date must be in ISO8601 format.
`x-siggy-date` has priority over `Date`

where KEYID is the id generated by siggy in the admin panel and HASH is created by the below method.


Create a string in the following format
```
$stringToSign = verb . "\n".
				path . "\n".
				timestamp . "\n".
				content_type . "\n".
				content_hash;
```

* verb - HTTP verb, uppercase format, GET,POST
* path - path relative to siggy's domain. i.e. /api/v1/systems/30000142
* timestamp - ISO8601 timestamp (same format as used in Date or x-siggy-date header)
* content_type - empty if GET request, otherwise the mime-type
* content_hash - empty if GET request, otherwise `base64_encode(sha256(content))`


The final HASH is then generated by

`HASH = base64_encode( hmac_sha256(stringToSign, KEYSECRET))`
