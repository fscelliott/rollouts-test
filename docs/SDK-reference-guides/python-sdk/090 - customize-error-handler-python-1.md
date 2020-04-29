---
title: "Customize error handler"
slug: "customize-error-handler-python-1"
hidden: true
createdAt: "2020-04-28T04:54:21.426Z"
updatedAt: "2020-04-28T15:09:21.910Z"
---
You can provide your own custom **error handler** logic to standardize across your production environment. 

This error handler is called when an unknown feature key is referenced.

See the code example below. If the error handler is not overridden, a no-op error handler is used by default.
[block:code]
{
  "codes": [
    {
      "code": "from optimizely.error_handler import NoOpErrorHandler as error_handler\nfrom optimizely import optimizely\n\noptimizely_client = optimizely.Optimizely(datafile,\n                                          event_dispatcher=event_dispatcher,\n                                          logger=logger,\n                                          error_handler=error_handler)\n\n",
      "language": "python"
    }
  ]
}
[/block]
To have finer grained control over your SDK configuration in a production environment, you can also pass in a custom error handler for your Optimizely client. A custom error handler can allow you to have more control over how you want to handle any errors coming from the Optimizely SDK.
[block:code]
{
  "codes": [
    {
      "code": "from optimizely import optimizely\nfrom optimizely import error_handler\n\n\nclass MyCustomErrorHandler(error_handler.BaseHandler):\n  \"\"\" Custom error handler class that extends and overrides the \n      handle_error method allowing you to define custom behavior. \n  \"\"\"\n  \n  @staticmethod\n  def handle_error(error):\n    # You can put custom logic here about how you want to handle the error.\n    # For example here we simply raise the error.\n    raise error\n    \n\n# Here we initialize the SDK with the custom error handler.\noptimizely_client = optimizely.Optimizely(datafile, error_handler=MyCustomErrorHandler)\n\n",
      "language": "python"
    }
  ]
}
[/block]