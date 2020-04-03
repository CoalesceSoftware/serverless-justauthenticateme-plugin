<p align="center">
  <a href="https://www.justauthenticate.me" target="_blank" rel="noopener noreferrer">
    <img width="100" src="https://www.justauthenticate.me/favicon.png" alt="JustAuthenticateMe logo">
  </a>
</p>
<p align="center">
  <a href="https://prettier.io/">
    <img alt="code style: prettier" src="https://badgen.net/badge/code style/prettier/ff69b4">
  </a>
  <a href="https://www.typescriptlang.org/">
    <img alt="types: typescript" src="https://badgen.net/badge/types/TypeScript/blue">
  </a>
</p>
<h1 align="center">JustAuthenticateMe Serverless Framework Plugin</h1>

## Introduction

[JustAuthenticateMe](https://www.justauthenticate.me) offers simple magic link based authentication as a
service for web apps. This is a serverless plugin that automatically authenticates your serverless
endpoints using JustAuthenticateMe. It uses the
[JustAuthenticateMe API Gateway Custom Authorizer](https://github.com/CoalesceSoftware/justauthenticateme-apigateway-auth)
to verify incoming requests and pass the user's email on to your endpoint handler.

## Supported Platforms

Currently, this plugin only supports AWS lambdas behind an API Gateway.

## Getting Started

### Installing via npm or yarn

`justauthenticateme-apigateway-auth` is a peer dependency so you'll have install it as well.

```
npm install --save serverless-justauthenticateme-plugin justauthenticateme-apigateway-auth
yarn add serverless-justauthenticateme-plugin justauthenticateme-apigateway-auth
```

### Adding to your `serverless.yml`

#### Step 1: Add the plugin

```yaml
plugins:
  - serverless-justauthenticateme-plugin
```

#### Step 2: Configure the plugin

You'll need your App ID from the JustAuthenticateMe console.

##### Static App ID

```yaml
custom:
  justauthenticateme:
    appId: 01234567-89ab-cdef-0123-4567890abcde
```

##### App ID per Stage

```yaml
custom:
  justauthenticateme:
    appId:
      production: 01234567-89ab-cdef-0123-4567890abcde
      staging: 456789ab-cdef-0123-4567-89abcdef0123
      dev: 890abcde-f012-3456-789a-bcdef1234567
```

#### Step 3: Specify Authenticated Endpoints

For each endpoint that should only be accessible by authenticated users, specify the authorizer as the
keyword `justauthenticateme` like so:

```yaml
functions:
  getBooks:
    handler: src/getBooks.handler
    events:
      - http:
          path: "api/books"
          method: get
          authorizer: justauthenticateme
          request:
            parameters:
              headers:
                Authorization: true
```

## Using the Authorizer

### Sending requests

When sending requests to endpoints that are protected by this authorizer, include the ID token you get
from JustAuthenticateMe in the `Authorization` header after the keyword `Bearer`. It should look something
like this:

```
Authorization: Bearer eyJhbGciOiJFUzUxMiIsInR5cCI6IkpXVCIsImtpZCI6IjJlYjQwMTA0LWRjNDUtNGYzNy1iNjljLTkzN2I2Mzg2YjlmNiJ9.eyJlbWFpbCI6InN1cHBvcnRAanVzdGF1dGhlbnRpY2F0ZS5tZSIsInN1YiI6InN1cHBvcnRAanVzdGF1dGhlbnRpY2F0ZS5tZSIsImF1ZCI6ImIxOWEyMWI0LWFkOWQtNGZkNy04OGMxLTFiNjhiODI1YzY3MSIsImlzcyI6Imh0dHBzOi8vZGV2LWFwaS5qdXN0YXV0aGVudGljYXRlLm1lL2IxOWEyMWI0LWFkOWQtNGZkNy04OGMxLTFiNjhiODI1YzY3MSIsImp0aSI6IjZhMjJjOTEyLWYwMzYtNGU0Mi1iZjM5LTQ3N2ZhM2ExOGY2ZCIsInRva2VuX3VzZSI6ImlkIiwiaWF0IjoxNTgzNjk1NDM5LCJuYmYiOjE1ODM2OTU0MzksImV4cCI6MTU4MzY5NzIzOX0.AZqvVWSXn4zwP4WhYOL-nQEDDEMa4Cmpyx8HGJ-6uc3wLeZVfvil6RyAlUExnd6JpteaAImOrKo5fnv93SSGkP-eAN9igGRg0GmXpIeGno_sY_4rMLXDa6RtABL1lz5LCYMxD79oIYIflWJ-LVqmCF90msq-PysFZcgKVLa8oki8ZlKI
```

### Handling requests

When a request is authenticated successfully, this lambda returns a policy allowing the user access to any
resource protected by this authorizer. It also passes along the email address of the authenticated user to
the handler of the API endpoint.

Specifically, a lambda handling an endpoint protected by this authorizer can access the user's email at
`event.requestContext.authorizer.email`.

## License

[MIT](http://opensource.org/licenses/MIT)
