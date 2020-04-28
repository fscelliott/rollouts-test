---
title: "Customize error handler"
slug: "customize-error-handler-ruby"
hidden: false
createdAt: "2019-08-21T21:53:08.590Z"
updatedAt: "2019-08-21T21:53:15.487Z"
---
You can provide your own custom **error handler** logic to standardize across your production environment. 

This error handler is called when an unknown feature key is referenced.

See the code example below. If the error handler is not overridden, a no-op error handler is used by default.
[block:code]
{
  "codes": [
    {
      "code": "# In a development environment, you might want the SDK to raise errors. In that case you can use out built-in RaiseErrorHandler\nerror_handler = Optimizely::RaiseErrorHandler.new\n\n# You can also define your own error handler\nclass CustomErrorHandler < Optimizely::BaseErrorHandler\n\n  def handle_error(error)\n    # You can handle this error in any way you'd like\n    puts error\n  end\n\nend\n\nerror_handler = CustomErrorHandler.new\n\n# Pass the error handler into the Optimizely instance\noptimizely_client = Optimizely::Project.new(datafile,\n                                            Optimizely::EventDispatcher.new,\n                                            Optimizely::NoOpLogger.new,\n                                            error_handler\n                                            )\n\n",
      "language": "ruby"
    }
  ]
}
[/block]