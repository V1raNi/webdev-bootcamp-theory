# Authentication

#### Table of Contents

- [Authentication](#authentication)
      - [Table of Contents](#table-of-contents)
  - [One Way Hashing](#one-way-hashing)
      - [Example of Hashing](#example-of-hashing)
    - [Sign In (Authentication)](#sign-in-authentication)
    - [JWT (JSON Web Token)](#jwt-json-web-token)
      - [Authentication](#authentication-1)
      - [Sending JWT to Server](#sending-jwt-to-server)

## One Way Hashing

It's converting data into a fixed length has string; we can only recreate the hash if we know the original data

Once we have the has, we can't go back and figure out what the original data was

It's applicable for saving passwords on our server - we never save actual password; the data that the site saves should be just hash and nothing else

#### Example of Hashing

Password 'password' -> given to a hash library 'bcrypt' -> $2a$10$9Mconplm8A780pY6iB2q.eBwkdldFbnz2tSH2uqHEi5B9KTpR3O8.

### Sign In (Authentication)

Once we've saved that password as a has, the process of signing is like this:

$2a$10$9Mconplm8A780pY6iB2q.eBwkdldFbnz2tSH2uqHEi5B9KTpR3O8. (in database) -> user provides a password ('password') -> password goes through 'bcrypt' -> hash is generated again -> compare those hashes -> log in

### JWT (JSON Web Token)

jwt.io

Users don't want to enter their passwords on every page; we need some proof that we have logged in in the past

We can use JWTs as proof that we've logged in before - JWT is a web standard for storing signed data (sometimes called 'jot')

JWT Format:

```
Header: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
Payload: eyJ1c2VySWQiOiIxMjM0In0.
Signature: kud-czcx6yOSSQgB0lKbibHNFmlAJwrV8iRQ1Ha-r-Q
```

Signature is used to make sure that the header and the payload has not been tempered with; both the header and the payload, and then also a secret key are inputs to generate the signature
- A secret key is a string that we store on the server but we don't give people access to it (use env variables, install dotenv for connecting to .env file)

The server is the one that keeps track of that secret key, and only it can know about the secret
- If a hacker were to get access to that key then the hacker could also make fake JWTs with whatever header and payload that they want
  - So the thing that keeps this secure is the fact that this signature generation can only be done by the server, it can't be done by the client of by anyone else

#### Authentication

The server verifies the payload and the header to make sure it hasn't been tempered with, and if it hasn't, it looks in the payload to see that it has something like a user ID stored, and then if that user ID is there, that verifies that we've logged in in the past

When the user signs in, we give the generated JWT back to the client; every authenticated request after that the client needs to send us the JWT back (if we're the server)
- As long as the client sends a JWT for which that signature matches the signature that we generate on the server, we are authenticated
- Once we look in this payload, we can see what the user ID is to figure out which user is authenticated
- If something is changed (header, payload, signature) it will lead to an invalid signature problem
  - If the header or payload are different, since they are inputs to generate signature, it will result in the server trying to generate a different signature

#### Sending JWT to Server

The standard way to send the JWT is to include the JWT in the authorization header, and the first part of this header is typically 'Bearer' (type portion of authorization)

HTTP Header: Authorization: Bearer \<JWT> (e.g. Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0In0.kud-czcx6yOSSQgB0lKbibHNFmlAJwrV8iRQ1Ha-r-Q)