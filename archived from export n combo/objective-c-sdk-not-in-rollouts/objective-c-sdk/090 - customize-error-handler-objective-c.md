---
title: "Customize error handler"
slug: "customize-error-handler-objective-c"
hidden: true
createdAt: "2019-09-12T15:59:19.004Z"
updatedAt: "2019-09-12T16:01:04.849Z"
---
You can provide your own custom **error handler** logic to standardize across your production environment. This error handler is called in the following situations:

* Unknown experiment key referenced
* Unknown event key referenced

See the code examples below. If the error handler is not overridden, a no-op error handler is used by default.
[block:code]
{
  "codes": [
    {
      "code": "/** \n * Provide your own custom error handler logic by replacing the\n * `errorHandler` property in the `OPTLYManagerBuilder` object\n * during initialization. The error handler you create must \n * conform to the `OPTLYErrorHandler` protocol. Overriding this\n * module will allow you to standardize error and exception \n * handling across your application.\n */\nCustomErrorHandler *customErrorHandler = [[CustomErrorHandler alloc] init];\n\n    OPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n  builder.sdkKey = @\"SDK_KEY_HERE\";\n  builder.errorHandler = customErrorHandler;\n}]];\n\n",
      "language": "objectivec"
    },
    {
      "code": "/**\n *  Provide your own custom error handler logic by replacing \n *  the `errorHandler` property in the `OPTLYManagerBuilder` \n *  object during initialization. The error handler you create\n *  must conform to the `OPTLYErrorHandler` protocol.  \n *  Overriding this module will allow you to standardize error\n *  and exception handling across your application.\n */\nlet customErrorHandler: CustomErrorHandler? = CustomErrorHandler()\n\nlet manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder!.sdkKey = \"SDK_KEY_HERE\"\n  builder!.errorHandler = customErrorHandler\n}))\n\n",
      "language": "swift"
    }
  ]
}
[/block]