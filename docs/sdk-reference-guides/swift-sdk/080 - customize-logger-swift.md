---
title: "Customize logger"
slug: "customize-logger-swift"
hidden: false
createdAt: "2019-08-21T21:58:53.695Z"
updatedAt: "2019-08-21T21:59:05.317Z"
---
The **logger** logs information about your experiments to help you with debugging. You can customize where log information is sent and what kind of information is tracked.

In the Swift SDK, you can use our default logger and configure `OptimizelyLogLevel`. You can initialize it with any log level and pass it in when creating an `OptimizelyClient` instance. You can also implement your own logger and pass it in the builder when creating an `OptimizelyClient` instance. See the code examples below. 
[block:code]
{
  "codes": [
    {
      "code": "class CustomLogger: OPTLogger {\n    public static var logLevel: OptimizelyLogLevel = .info\n\n    required init() {\n    }\n\n    public func log(level: OptimizelyLogLevel, message: String) {\n        if level.rawValue <= CustomLogger.logLevel.rawValue {\n            print(\"\\(message)\")\n        }\n    }\n}\n\n// create OptimizelyClient with a custom Logger\nlet customLogger = CustomLogger()\n\noptimizely = OptimizelyClient(sdkKey: sdkKey,\n                              logger: customLogger,\n                              defaultLogLevel:.debug)\n\n",
      "language": "swift"
    },
    {
      "code": "@interface CustomLogger : NSObject <OPTLogger>\n+ (enum OptimizelyLogLevel)logLevel;\n+ (void)setLogLevel:(enum OptimizelyLogLevel)value;\n- (nonnull instancetype)init;\n- (void)logWithLevel:(enum OptimizelyLogLevel)level message:(NSString *)message;\n@end\n  \n@implementation CustomLogger\n-(instancetype)init {\n    self = [super init];\n    if (self != nil) {\n        //\n    }\n    \n    return self;\n}\n\n- (void)logWithLevel:(enum OptimizelyLogLevel)level message:(NSString * _Nonnull)message {\n    if (level <= CustomLogger.logLevel) {\n        NSLog(@\"%@\", message);\n    }\n}\n\nstatic enum OptimizelyLogLevel logLevel = OptimizelyLogLevelInfo;\n+(enum OptimizelyLogLevel)logLevel {\n    return logLevel;\n}\n+(void)setLogLevel:(enum OptimizelyLogLevel)value {\n    logLevel = value;\n}\n@end\n\n\n// create OptimizelyClient with a custom Logger\nCustomLogger *customLogger = [[CustomLogger alloc] init];\n     \nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:kOptimizelySdkKey\n                            \t\t\t\tlogger:customLogger\n                            \t\t\t\teventDispatcher:nil\n                            \t\t\t\tuserProfileService:nil\n                            \t\t\t\tperiodicDownloadInterval:5*60\n                            \t\t\t\tdefaultLogLevel:OptimizelyLogLevelInfo];\n\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Log levels"
}
[/block]
The table below lists the log levels for the Swift SDK.
[block:parameters]
{
  "data": {
    "h-0": "Log Level",
    "h-1": "Explanation",
    "0-0": "**OptimizelyLogLevel.error**",
    "0-1": "Events that prevent feature flags from functioning correctly (for example, invalid datafile in initialization and invalid feature keys) are logged. The user can take action to correct.",
    "1-0": "**OptimizelyLogLevel.warning**",
    "1-1": "Events that don't prevent feature flags from functioning correctly, but can have unexpected outcomes (for example, future API deprecation, logger or error handler are not set properly, and nil values from getters) are logged.",
    "2-0": "**OptimizelyLogLevel.info**",
    "2-1": "Events of significance (for example, activate started, activate succeeded, tracking started, and tracking succeeded) are logged. This is helpful in showing the lifecycle of an API call.",
    "3-0": "**OptimizelyLogLevel.debug**",
    "3-1": "Any information related to errors that can help us debug the issue (for example, the feature flag is not running, user is not included in the rollout) are logged."
  },
  "cols": 2,
  "rows": 4
}
[/block]