---
title: "Get the datafile"
slug: "datafile"
hidden: false
createdAt: "2019-08-21T19:32:16.478Z"
updatedAt: "2020-02-27T18:46:40.329Z"
---
In Optimizely, the datafile is a JSON representation of all the feature flags, audiences, environments, and other elements that you’ve configured in your project.

The datafile is updated immediately after changes to your configuration, but it may take a few minutes to upload to the content delivery network (CDN). In general, you don’t need to access the datafile, but it can be handy if your features don’t update as expected. The datafile can also help you debug and confirm expected updates.

See also [best practices for managing the datafile](doc:manage-the-datafile).
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "If you're using the Android or Swift SDKs, skip straight to Initialize ([Android](doc:initialize-sdk-android) or [Swift](doc:initialize-sdk-swift)), which encompasses getting the datafile and instantiating a client."
}
[/block]

[block:api-header]
{
  "title": "Get the datafile"
}
[/block]
Before you instantiate a client, get a local copy of the datafile from our servers. Synchronizing this local copy lets the SDK make immediate decisions on features, without waiting for a blocking network request.
[block:code]
{
  "codes": [
    {
      "code": "string datafile = string.Empty;\nvar url = \"https://cdn.optimizely.com/json/12345.json\";\nusing(var webClient = new System.Net.WebClient()){\n\n  // To refresh on every request\n  webClient.CachePolicy = new System.Net.Cache.RequestCachePolicy(System.Net.Cache.RequestCacheLevel.NoCacheNoStore);\n  datafile = webClient.DownloadString(url);\n}\n\n",
      "language": "csharp",
      "name": "C#"
    },
    {
      "code": "import org.apache.http.client.methods.HttpGet;\nimport org.apache.http.impl.client.CloseableHttpClient;\nimport org.apache.http.impl.client.HttpClients;\nimport org.apache.http.util.EntityUtils;\n\ntry {\n    String url = \"https://cdn.optimizely.com/datafiles/QMVJcUKEJZFg8pQ2jhAybK.json\";\n    CloseableHttpClient httpclient = HttpClients.createDefault();\n    HttpGet httpget = new HttpGet(url); \n    String datafile = EntityUtils.toString(\n      httpclient.execute(httpget).getEntity());\n}\ncatch (Exception e) {\n    System.out.print(e);\n    return;\n}\n\n",
      "language": "java",
      "name": "Java"
    },
    {
      "code": "<!-- Add /tag.js to the end of your datafile URL --> \n<script src=\"https://cdn.optimizely.com/datafiles/<Your_SDK_KEY>.json/tag.js\"></script>\n<script>\n   // Datafile is available on the window variable\n   console.log(window.optimizelyDatafile)\n</script>\n\n",
      "language": "html"
    },
    {
      "code": "const url = 'https://cdn.optimizely.com/json/12345.json';\nwindow\n  .fetch(url, { mode: 'cors' })\n  .then(response => response.json())\n  .then(datafile => {\n    const optimizelyClientInstance = optimizelySdk.createInstance({\n      datafile,\n    });\n  });\n\n",
      "language": "javascript"
    },
    {
      "code": "// Minimal client\nconst optimizely = require(\"@optimizely/optimizely-sdk\");\nconst rp = require(\"request-promise\");\n\n// TODO: replace with your datafile URL\nconst DATAFILE_URL =\n  \"https://cdn.optimizely.com/datafiles/QMVJcUKEJZFg8pQ2jhAybK.json\";\nconst datafile = await rp({ uri: DATAFILE_URL, json: true });\nconsole.log(\"Datafile:\", datafile);\nvar optimizelyClient = optimizely.createInstance({\n  datafile\n});\n// Use optimizelyClient to run experiments\n\n",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "use Optimizely\\Optimizely;\n\n$url = 'https://cdn.optimizely.com/datafiles/abcdEFGH123.json';\n$datafile = file_get_contents($url);\n\n// Instantiate an Optimizely client\n$optimizelyClient = new Optimizely($datafile);\n\n",
      "language": "php",
      "name": "PHP"
    },
    {
      "code": "from optimizely import optimizely\nimport requests\n\nurl = 'https://cdn.optimizely.com/datafiles/abcdEFGH123.json'\ndatafile = requests.get(url).text\noptimizely_client = optimizely.Optimizely(datafile)\n\n",
      "language": "python"
    },
    {
      "code": "require 'optimizely'\nrequire 'httparty'\n\nurl = 'https://cdn.optimizely.com/datafiles/QMVJcUKEJZFg8pQ2jhAybK.json'\ndatafile = HTTParty.get(url).body\noptimizely_client = Optimizely::Project.new(datafile)\n\n",
      "language": "ruby"
    }
  ]
}
[/block]
How you synchronize and how often you synchronize depends on your application:
* **Recommended method**: Use [webhooks](doc:webhooks) to notify your server when the datafile changes so it can retrieve the latest version. 
* **Alternative method**: Poll the CDN or REST API at a regular interval (such as every minute) for an updated datafile. The length of this interval will vary according to your application  requirements.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Be sure to synchronize the datafile often enough to stay current with changes in the Optimizely project environment without generating unacceptable network traffic and latency. For more information on how to balance network latency with datafile freshness, read about [datafile versioning and management](doc:manage-the-datafile)."
}
[/block]
Once you have a datafile, you can instantiate an Optimizely client. Any time you retrieve an updated datafile, just re-instantiate the same client.

For simple applications, all you need to provide to instantiate a client is a datafile specifying the project configuration for a given environment. For most advanced implementations, you'll want to customize the client for your specific requirements. See the Instantiate method documentation for more information.
[block:api-header]
{
  "title": "Fetch the datafile via REST API"
}
[/block]
You can access the datafile via Optimizely's authenticated REST API. Note that you'll need to [authenticate with an API token](https://developers.optimizely.com/x/rest/conventions/#authenticating). For more information, see [Read the datafile of an environment](https://developers.optimizely.com/x/rest/v2/#read-the-datafile-of-an-environment).
[block:code]
{
  "codes": [
    {
      "code": "https://api.optimizely.com/v2/projects/12345",
      "language": "http"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Access the datafile via the app"
}
[/block]
To access the datafile for your Full Stack project:
1. Navigate to _Settings > Datafile_.
2. Find the SDK Key/Primary URL for the project and environment whose datafile you want to access.
3. Click the SDK Key/Primary URL to open the CDN link in a new browser tab or window.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ae495fd-datafile-open.gif",
        "datafile-open.gif",
        1485,
        837,
        "#f9fafb"
      ],
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Take care when changing the datafile's environment key as it will affect the datafile URL your code loads.",
  "title": "Important"
}
[/block]

[block:api-header]
{
  "title": "View the datafile via the CDN link"
}
[/block]
This image shows how the datafile appears when accessed from the CDN link.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8ff5fe6-datafile.png",
        "datafile.png",
        1344,
        660,
        "#e0dfdf"
      ],
      "border": true
    }
  ]
}
[/block]
View an [example datafile](doc:example-datafile) in properly formatted JSON.