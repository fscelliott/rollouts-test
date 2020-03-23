---
title: "Install SDK"
slug: "install-sdk-java"
hidden: false
createdAt: "2019-08-21T21:23:16.677Z"
updatedAt: "2019-08-21T21:23:30.954Z"
---
The Java SDK is distributed through Bintray. The `core-api` and `httpclient` Bintray packages are [optimizely-sdk-core-api](https://bintray.com/optimizely/optimizely/optimizely-sdk-core-api) and [optimizely-sdk-httpclient](https://bintray.com/optimizely/optimizely/optimizely-sdk-httpclient),  respectively. You can find repository information and also instructions on how to install the dependencies on Bintray. Gradle repository and dependency configurations are shown on the right. The SDK is compatible with Java 1.6 and above.

`core-api` requires `org.slf4j:slf4j-api:1.7.16` and a supported JSON parser. We currently integrate with these parsers (listed in their selection order priority): 
1. [Jackson](https://github.com/FasterXML/jackson)
2. [GSON](https://github.com/google/gson)
3. [json.org](http://www.json.org/)
4. [json-simple](https://code.google.com/archive/p/json-simple/)

If more than one of these parsers are available at runtime, the `core-api` chooses one for use per the selection order priority. If none of these packages are already provided in your project's classpath, you must add one.
[block:code]
{
  "codes": [
    {
      "code": "repositories {\n    jcenter()\n}\n\ndependencies {\n    compile 'com.optimizely.ab:core-api:3.0.0'\n    compile 'org.apache.httpcomponents:httpclient:4.5.6'\n\n    // The SDK integrates with multiple JSON parsers. Here, we use Jackson.\n    compile 'com.fasterxml.jackson.core:jackson-core:2.7.1'\n    compile 'com.fasterxml.jackson.core:jackson-annotations:2.7.1'\n    compile 'com.fasterxml.jackson.core:jackson-databind:2.7.1'\n\n    compile 'org.slf4j:slf4j-api:1.7.16'\n    compile 'ch.qos.logback:logback-classic:1.1.7'\n}\n\n",
      "language": "java"
    }
  ]
}
[/block]

The full source code is at https://github.com/optimizely/java-sdk.