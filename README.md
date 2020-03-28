<p align="center"><a href="https://www.justauthenticate.me" target="_blank" rel="noopener noreferrer"><img width="100" src="https://www.justauthenticate.me/favicon.png" alt="JustAuthenticateMe logo"></a></p>
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

[JustAuthenticateMe](https://www.justauthenticate.me) offers simple magic link based authentication as a service for web apps. This is a serverless plugin that automatically authenticates your serverless endpoints using JustAuthenticateMe. It uses the [JustAuthenticateMe API Gateway Custom Authorizer](https://https://github.com/CoalesceSoftware/justauthenticateme-apigateway-auth) to verify incoming requests and pass the user's email on to your endpoint handler.

## Support

Currently, this plugin only supports AWS lambdas behind an API Gateway.

## Getting Started

### Installing via npm or yarn

```
npm install --save serverless-justauthenticateme-plugin
yarn add serverless-justauthenticateme-plugin
```

### Usage

```js
TODO;
```

## How it works

TODO how it passes email to actual endpoint lambda

## License

[MIT](http://opensource.org/licenses/MIT)
