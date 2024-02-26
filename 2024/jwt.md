# How to Mess Up Json Web Tokens

- Claims
    - Name value pair with information about something
    - JWT can contain arbitrary claims
    - Spec defines certain claims (`iss`, `sub`, `aud`, `exp`, etc)
- JWT: 3 base 64 encoded sections separated by dots
    - header: `alg: HS256` & `typ: JWT`
    - claims: `sub`, `name`, `aud`, `iat`
    - signature
- Using JWT: client sends access token to server app, auth server sends it and validates it
- jwt.ms client-app, maybe better than jwt.io
- Tokens are usually not encrypted but signed
- Signing can use a shared key, or public/private key pair
- Token have an expiry date, but cannot be invalidated
- Tokens are meant for certain systems (audiences)

## Unsigned Token

- Unsigned token (removed third part), `alg` = none
- Validation passes because no signature expected
- Can claim anything

Do not accept `none`

## Self-signed Tokens

Add `jwk` in the header, provide public key for signature. Server may verify with the user provided public key.

## Reused Tokens

Tokens can be reused. Common pattern: use token once, issue session ID.
Refresh tokens can be provided in the login endpoint, can be stolen and reuse. Need rotation.
Strategies include deny list for token, bind token to a browser

## Insecure Token Storage

If token is in local storage, HTTP requests send it. Susceptible to XSS.
Solution: use sessions with secure cookies.
Use BFF (backend for frontend) in local storage

## Other issues

- Sensitive cleartext information in tokens (roles in clear text)
- Bloated tokens: performance issue, sent with every request
- Ignoring token audience: test & prod environments might be mixed
- Expiration date ignored or too far in the future. In frameworks, use decode & verify functions.
- If secret is too short, can be brute forced