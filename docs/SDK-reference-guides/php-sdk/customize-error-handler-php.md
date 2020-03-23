---
title: "Customize error handler"
slug: "customize-error-handler-php"
hidden: false
createdAt: "2019-08-21T21:43:16.651Z"
updatedAt: "2019-08-21T21:43:21.708Z"
---
You can provide your own custom **error handler** logic to standardize across your production environment. 

This error handler is called when an unknown feature key is referenced.

See the code example below. If the error handler is not overridden, a no-op error handler is used by default.
[block:code]
{
  "codes": [
    {
      "code": "use Optimizely\\ErrorHandler\\DefaultErrorHandler;\n\n$optimizelyClient = new Optimizely($datafile, null, null, new DefaultErrorHandler());\n\n",
      "language": "php"
    }
  ]
}
[/block]
To have finer grained control over your SDK configuration in a production environment, you can also pass in a custom error handler for your Optimizely client. A custom error handler can allow you to have more control over how you want to handle any errors coming from the Optimizely SDK.
[block:code]
{
  "codes": [
    {
      "code": "use Optimizely\\ErrorHandler\\ErrorHandlerInterface;\n\n/**\n * Class MyCustomErrorHandler\n *  \n * Custom error handler class that extends and overrides the \n * handleError method allowing you to define custom behavior.\n *\n */\nclass MyCustomErrorHandler implements ErrorHandlerInterface\n{\n    public function handleError(Exception $error)\n    {\n       // You can put custom logic here about how you want to handle the error.\n       // For example here we simply throw the error.       \n       throw $error;\n    }\n}\n\n// This error handler can then be used when initializing the Optimizely SDK.\n\n$optimizelyClient = new Optimizely($datafile, null, null, new MyCustomErrorHandler());\n\n",
      "language": "php"
    }
  ]
}
[/block]