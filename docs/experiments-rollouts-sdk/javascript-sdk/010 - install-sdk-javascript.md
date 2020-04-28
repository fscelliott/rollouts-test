---
title: "Install SDK"
slug: "install-sdk-javascript"
hidden: false
createdAt: "2019-08-21T21:32:37.686Z"
updatedAt: "2020-04-14T22:25:26.918Z"
---
The JavaScript SDK can be used in browser and is distributed through [npm](https://www.npmjs.com/package/@optimizely/optimizely-sdk). Add it to your package with NPM:
[block:code]
{
  "codes": [
    {
      "code": "npm install --save @optimizely/optimizely-sdk\n\n",
      "language": "text",
      "name": "Using NPM"
    }
  ]
}
[/block]
Or using Yarn:
[block:code]
{
  "codes": [
    {
      "code": "yarn add @optimizely/optimizely-sdk\n\n",
      "language": "text",
      "name": "Using Yarn"
    }
  ]
}
[/block]
We also distribute a prebuilt UMD bundle of the JavaScript SDK, which allows installing the SDK via an HTML <script> tag, which makes the SDK available under the global variable `optimizelySdk`
[block:code]
{
  "codes": [
    {
      "code": "<script src=\"https://unpkg.com/@optimizely/optimizely-sdk@3.5/dist/optimizely.browser.umd.min.js\"></script>\n\n",
      "language": "html",
      "name": "Using an HTML script tag"
    }
  ]
}
[/block]
The full source code is at https://github.com/optimizely/javascript-sdk/tree/master/packages/optimizely-sdk.