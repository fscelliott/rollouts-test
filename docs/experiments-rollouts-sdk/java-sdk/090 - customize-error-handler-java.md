---
title: "Customize error handler"
slug: "customize-error-handler-java"
hidden: false
createdAt: "2019-08-21T21:27:05.165Z"
updatedAt: "2019-08-21T21:27:28.622Z"
---
You can provide your own custom **error handler** logic to standardize across your production environment. 

This error handler is called when an unknown feature key is referenced.

Here is a code example where an error handler from the SDK is used. If the error handler is not overridden, a no-op error handler is used by default.
[block:code]
{
  "codes": [
    {
      "code": "import com.optimizely.ab.Optimizely;\nimport com.optimizely.ab.error.ErrorHandler;\nimport com.optimizely.ab.error.RaiseExceptionErrorHandler;\nimport com.optimizely.ab.event.AsyncEventHandler;\nimport com.optimizely.ab.event.EventHandler;\n\nEventHandler eventHandler = new AsyncEventHandler(20000, 1);\n\n// Default handler that raises exceptions\nErrorHandler errorHandler = new RaiseExceptionErrorHandler();\n\nOptimizely optimizelyClient;\ntry {\n    // Create an Optimizely client with the default event dispatcher\n    optimizelyClient = Optimizely.builder(datafile, eventHandler)\n                           .withErrorHandler(errorHandler)\n                           .build();\n} catch (ConfigParseException e) {\n    // Handle exception gracefully\n    return;\n}\n\n",
      "language": "java"
    }
  ]
}
[/block]
For additional control and visibility into the errors coming from the Optimizely SDK, we recommend implementing your own custom error handler. With a custom error handler, you can choose what to do with an error, whether it may be as simple as logging the error to console or sending the error to another error monitoring service. Below is an example of using a custom error handler to log errors:
[block:code]
{
  "codes": [
    {
      "code": "import com.optimizely.ab.Optimizely;\nimport com.optimizely.ab.error.ErrorHandler;\nimport com.optimizely.ab.OptimizelyRuntimeException;\nimport org.slf4j.Logger;\nimport org.slf4j.LoggerFactory;\n\n// Custom handler that logs the errors\npublic class CustomErrorHandler implements ErrorHandler {\n  \tprivate static final Logger logger = LoggerFactory.getLogger(CustomErrorHandler.class);\n  \n    @Override\n    public <T extends OptimizelyRuntimeException> void handleError(T exception) {\n        logger.error(exception.getMessage());\n    }\n}\n\nEventHandler eventHandler = new AsyncEventHandler(20000, 1);\nErrorHandler customErrorHandler = new CustomErrorHandler();\n\nOptimizely optimizelyClient;\ntry {\n    // Create an Optimizely client with the default event dispatcher\n    optimizelyClient = Optimizely.builder(datafile, eventHandler)\n                           .withErrorHandler(customErrorHandler)\n                           .build();\n} catch (ConfigParseException e) {\n    // Handle exception gracefully\n    return;\n}\n\n",
      "language": "java"
    }
  ]
}
[/block]