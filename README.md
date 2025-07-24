# ClassLink User Enumeration Vulnerability (Unauthenticated)

## Summary

A user enumeration vulnerability was discovered in the ClassLink LaunchPad password reset feature. By sending a `POST` request to the `/user/resetpassword/options` endpoint with different usernames, it is possible to distinguish valid accounts based on the application's response. This behavior allows attackers to enumerate valid usernames, which could be used in further attacks like credential stuffing or phishing.

## Affected System

- **Product:** ClassLink LaunchPad (SSO Portal)
- **Vendor:** ClassLink, Inc.
- **Tested As Of:** July 2025
- **Version:** Unknown (cloud-hosted)

## Vulnerability Details

### Endpoint:
POST /user/resetpassword/options

Expected: JSON response containing a verificationToken.

curl -X POST https://<target-domain>/user/resetpassword/options \
     -d "username=invaliduser@domain.com" \
     -H "Content-Type: application/x-www-form-urlencoded"

Expected: "Password Recovery is not set up"

Impact

    Allows attackers to discover valid usernames.

    Increases risk of password spraying, credential stuffing, and phishing attacks.

    Particularly sensitive due to use in K–12 educational environments.

Mitigation

The server should return the same generic response for both valid and invalid usernames to prevent enumeration.
Disclosure

    Reported by: KJ Morrison (Independent Security Researcher)


