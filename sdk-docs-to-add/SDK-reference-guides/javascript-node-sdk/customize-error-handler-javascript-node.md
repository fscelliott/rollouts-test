---
title: "Customize error handler"
slug: "customize-error-handler-javascript-node"
hidden: false
createdAt: "2019-08-21T21:38:37.105Z"
updatedAt: "2019-08-21T21:38:42.496Z"
---
In a production environment, you'll want to have full control and visibility over the errors that are happening in your application, including those that would originate from an Optimizely SDK.
 
The Optimizely SDKs provide default implementations of an error handler in the SDKs. Below is an example of using the default error handler from the SDKs:
[block:code]
{
  "codes": [
    {
      "code": "const OptimizelySdk = require(\"@optimizely/optimizely-sdk\");\n \n// 3.0.0 SDK\nvar defaultErrorHandler = require(\"@optimizely/optimizely-sdk/lib/plugins/error_handler\");\n \n// 3.0.1 SDK and above\nvar defaultErrorHandler = require(\"@optimizely/optimizely-sdk\").errorHandler;\n \nOptimizelySdk.setLogger(OptimizelySdk.logging.createLogger())\n \nconst optimizelyClient = OptimizelySdk.createInstance({\n  // No datafile will trigger the custom error handler,\n  // but the default error handler is a no-op\n  datafile: null,\n  errorHandler: defaultErrorHandler\n});\n",
      "language": "javascript"
    }
  ]
}
[/block]
However, for additional control and visibility into the errors coming from the Optimizely SDK, we recommend implementing your own custom error handler.
 
With a custom error handler, you can choose what to do with an error, whether it may be as simple as logging the error to console or sending the error to another error monitoring service.
 
Below is an example of using a custom error handler to log errors to the console:
[block:code]
{
  "codes": [
    {
      "code": "const OptimizelySdk = require(\"@optimizely/optimizely-sdk\");\n \n/**\n * customErrorHandler\n *\n * Object that has a property `handleError` which will be called\n * when an error is thrown in the SDK.\n */\nconst customErrorHandler = {\n  /**\n   * handleError\n   *\n   * Function which gets called when an error is thrown in the SDK\n   * @param {Object} error - error object\n   * @param {String} error.message - message of the error\n   * @param {String} error.stack - stack trace for the error\n   */\n  handleError: function(error) {\n    console.log('CUSTOM_ERROR_HANDLER');\n    console.log('****');\n    console.log(`Error Message: ${error.message}`);\n    console.log(`Error Stack: ${error.stack}`);\n    console.log('****');\n  }\n}\n \nOptimizelySdk.setLogger(OptimizelySdk.logging.createLogger())\n \nconst optimizelyClientInstance = OptimizelySdk.createInstance({\n  // No datafile will trigger the custom error handler,\n  datafile: null,\n  errorHandler: customErrorHandler,\n});\n\n",
      "language": "javascript"
    }
  ]
}
[/block]