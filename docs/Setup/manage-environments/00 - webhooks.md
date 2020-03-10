---
title: "Create webhooks"
slug: "webhooks"
hidden: false
createdAt: "2019-08-21T19:32:16.446Z"
updatedAt: "2019-09-27T21:56:04.760Z"
---
To get the most value when using Optimizely, you'll want changes made to your feature flags and rollouts in the Optimizely UI to take effect in your applications as soon as possible.

One way to do this is to have your application poll for changes in the datafile that contains all the latest information on your feature flags and rollouts. However, if you're using Optimizely in an application with a server-side component, we recommend configuring a webhook.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "Optimizely supports webhooks only for the primary environment.\n\nOptimizely sends a POST request to your supplied endpoint when you make a change in any environment.\n\nWhenever you make a change to an environment, Optimizely updates the revision of each of your environments' datafiles. For example, if you have four environments and make a change in one of them, the revisions of all four of your environments’ datafiles are updated. Although your webhook is only triggered when a change is made to your primary environment, a change to any environment causes a change to your primary environment’s revision and triggers a webhook notification."
}
[/block]
By using a webhook, Optimizely can send a notification to your server when the datafile updates, eliminating the need to constantly poll and determine if there are changes in the Optimizely configuration. Once you receive the webhook and download the latest datafile, you must re-instantiate the Optimizely client with the latest datafile for the changes to take effect as soon as possible.

Follow the instructions below to setup a webhook and test that it is working correctly.
[block:api-header]
{
  "title": "1. Set up an endpoint on your server"
}
[/block]
The first step to setting up a webhook is to set up a public API endpoint on your server, which can accept a POST request from Optimizely.

We recommend naming your endpoint `/webhooks/optimizely`. However, you can name the endpoint anything you'd like.

Setting up a public endpoint on your server varies greatly depending on the language and framework that you use. The examples below show Node (JavaScript) using Express and Python using Flask.
[block:code]
{
  "codes": [
    {
      "code": "// Simple Node Express webhook example (see full example in step 3)\n// Requires installing express: 'npm install --save express'\nconst express = require('express');\nconst app = express();\n\n/**\n * Optimizely Webhook Route\n * Route to accept webhook notifications from Optimizely\n **/\napp.use('/webhooks/optimizely', (req, res, next) => {\n  console.log(`\n    [Optimizely] Optimizely webhook request received!\n    The Optimizely datafile has been updated. Re-download\n    the datafile and re-instantiate the Optimizely SDK\n    for the changes to take effect\n  `);\n  res.send('Webhook Received');\n});\n\napp.get('/', (req, res) => res.send('Optimizely Webhook Example'))\n\nconst HOST = process.env.HOST || '0.0.0.0';\nconst PORT = process.env.PORT || 8080;\napp.listen(PORT, HOST);\nconsole.log(`Example App Running on http://${HOST}:${PORT}`);\n\n",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "# Simple Python Flask webhook example (see full example in step 3)\n# Requires installing flask: 'pip install flask'\nimport os\nfrom flask import Flask, request, abort\n\napp = Flask(__name__)\n\n# Route to accept webhook notifications from Optimizely\n@app.route('/webhooks/optimizely', methods=['POST'])\ndef index():\n  print(\"\"\"\n    [Optimizely] Optimizely webhook request received!\n    The Optimizely datafile has been updated. Re-download\n    the datafile and re-instantiate the Optimizely SDK\n    for the changes to take effect\n  \"\"\")\n  return 'Webhook Received'\n\n@app.route('/')\ndef hello_world():\n  return 'Optimizely Webhook Example'\n\nif __name__ == \"__main__\":\n  host = os.getenv('HOST', '0.0.0.0')\n  port = int(os.getenv('PORT', 3000))\n  print('Example App unning on http://' + str(port) + ':' + str(host))\n  app.run(host=host, port=port)\n\n",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "2. Create a webhook in Optimizely"
}
[/block]
Once you've setup an endpoint on your server, take note of the fully qualified URL of the endpoint you created on your server. If your server domain is `https://www.your-example-site.com` and you named your endpoint `/webhooks/optimizely` as described in step 1, then your fully qualified webhook URL is `https://www.your-example-site.com/webhooks/optimizely`. Use this URL in the steps below:

1. Navigate to _Settings > Webhooks_.
2. Click _Create New Webhook..._
3. Enter the URL where Optimizely will send datafile update notifications (example: `https://www.your-example-site.com/webhooks/optimizely`)
4. Click _Save_.
5. Take note of the secret generated to secure your webhook in the next step.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b5ab845-webhook.gif",
        "webhook.gif",
        1466,
        807,
        "#f8f9fb"
      ],
      "border": true
    }
  ]
}
[/block]
The example below shows the webhook payload structure with the default Optimizely primary `environment` of "Production"&mdash;your `environment` value may be different. Currently, we support one event type: `project.datafile_updated`.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"project_id\": 1234,\n  \"timestamp\": 1468447113,\n  \"event\": \"project.datafile_updated\",\n  \"data\": {\n    \"revision\": 1,\n    \"origin_url\": \"https://optimizely.s3.amazonaws.com/json/1234.json\",\n    \"cdn_url\": \"https://cdn.optimizely.com/json/1234.json\",\n    \"environment\": \"Production\"\n  }\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "3. Secure your webhook"
}
[/block]
Once you've set up a public endpoint that can accept datafile update notifications, you'll want to secure your webhook to ensure that these notifications are actually coming from Optimizely and not from some malicious user trying to control your server's feature flag configuration.

When you create a webhook, Optimizely generates a secret token that is used to create a hash signature of webhook payloads. Webhook requests include this signature in a header `X-Hub-Signature` that can be used to verify the request originated from Optimizely.

You can only view a webhook's secret token once, immediately after its creation. If you forget a webhook's secret token, you must regenerate it on the [_Settings > Webhook_](#section-2-create-a-webhook-in-optimizely) page.

The `X-Hub-Signature` header contains a SHA1 HMAC hexdigest of the webhook payload, using the webhook's secret token as the key and prefixed with `sha1=`. The way you verify this signature varies, depending on the language of your codebase. The reference implementation examples below show Node (JavaScript) using Express and Python using Flask.

Both examples assume your webhook secret is passed as an environment variable named `OPTIMIZELY_WEBHOOK_SECRET`.

[block:code]
{
  "codes": [
    {
      "code": "// Simple Node Express webhook example (see full example in step 3)\n// Requires installing express: 'npm install --save express body-parser'\nconst express = require('express');\nconst bodyParser = require('body-parser')\nconst crypto = require('crypto');\nconst app = express();\n\n/**\n * Optimizely Webhook Route\n * Route to accept webhook notifications from Optimizely\n **/\napp.use('/webhooks/optimizely', bodyParser.text({ type: '*/*' }), (req, res, next) => {\n\n  const WEBHOOK_SECRET = process.env.OPTIMIZELY_WEBHOOK_SECRET\n  const webhook_payload = req.body\n  const hmac = crypto.createHmac('sha1', WEBHOOK_SECRET)\n  const webhookDigest = hmac.update(webhook_payload).digest('hex')\n\n  const computedSignature = `sha1=${webhookDigest}`\n  const requestSignature = req.header('X-Hub-Signature')\n\n  if (!crypto.timingSafeEqual(Buffer.from(computedSignature, 'utf8'), Buffer.from(requestSignature, 'utf8'))) {\n    console.log(`[Optimizely] Signatures did not match! Do not trust webhook request\")`)\n    res.status(500)\n    return\n  }\n\n  console.log(`\n    [Optimizely] Optimizely webhook request received!\n    Signatures match! Webhook verified as coming from Optimizely\n    Download Optimizely datafile and re-instantiate the SDK Client\n    For the latest changes to take affect\n  `);\n  res.sendStatus(200)\n});\n\napp.get('/', (req, res) => res.send('Optimizely Webhook Example'))\n\nconst HOST = process.env.HOST || '0.0.0.0';\nconst PORT = process.env.PORT || 8080;\napp.listen(PORT, HOST);\nconsole.log(`Example App Running on http://${HOST}:${PORT}`);\n\n",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "# Reference Flask implementation of secure webhooks\n# Requires installing flask: 'pip install flask'\n# Assumes webhook's secret is stored in the environment variable OPTIMIZELY_WEBHOOK_SECRET\nfrom hashlib import sha1\nimport hmac\nimport os\n\nfrom flask import Flask, request, abort\n\napp = Flask(__name__)\n\n# Route to accept webhook notifications from Optimizely\n@app.route('/webhooks/optimizely', methods=['POST'])\ndef index():\n  request_signature = request.headers.get('X-Hub-Signature')\n  webhook_secret = bytes(os.environ['OPTIMIZELY_WEBHOOK_SECRET'], 'utf-8')\n  webhook_payload = request.data\n  digest = hmac.new(webhook_secret, msg=webhook_payload, digestmod=sha1).hexdigest()\n  computed_signature = 'sha1=' + str(digest)\n\n  if not hmac.compare_digest(computed_signature, request_signature):\n    print(\"[Optimizely] Signatures did not match! Do not trust webhook request\")\n    abort(500)\n    return\n\n  print(\"\"\"\n    [Optimizely] Optimizely webhook request received!\n    Signatures match! Webhook verified as coming from Optimizely\n    Download Optimizely datafile and re-instantiate the SDK Client\n    For the latest changes to take affect\n  \"\"\")\n  return 'Secure Webhook Received'\n\n@app.route('/')\ndef hello_world():\n  return 'Optimizely Webhook Example'\n\nif __name__ == \"__main__\":\n  host = os.getenv('HOST', '0.0.0.0')\n  port = int(os.getenv('PORT', 3000))\n  print('Example App unning on http://' + str(port) + ':' + str(host))\n  app.run(host=host, port=port)\n\n",
      "language": "python"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Be sure to keep all webhook secret tokens for your implementation secure and private."
}
[/block]

[block:callout]
{
  "type": "danger",
  "body": "To prevent timing analysis attacks, we strongly recommend that you use a constant time string comparison function such as Python's [`hmac.compare_digest`](https://docs.python.org/2/library/hmac.html#hmac.compare_digest) or Rack's [`secure_compare`](http://www.rubydoc.info/github/rack/rack/master/Rack/Utils.secure_compare) instead of the `==` operator when verifying webhook signatures.",
  "title": "Warning"
}
[/block]

[block:api-header]
{
  "title": "4. Test your implementation"
}
[/block]
To help ensure that your secure webhook implementation is correct, below are example values that you can use to test your implementation. These test values are also used in the running Node example in [Runkit](https://runkit.com/asaschachar/secure-webhook-example-node).

### Example webhook secret token
```
yIRFMTpsBcAKKRjJPCIykNo6EkNxJn_nq01-_r3S8i4
```

### Example webhook request payload
```
'{"timestamp": 1558138293, "project_id": 11387641093, "data": {"cdn_url": "https://cdn.optimizely.com/datafiles/QMVJcUKEJZFg8pQ2jhAybK.json", "environment": "Production", "origin_url": "https://optimizely.s3.amazonaws.com/datafiles/QMVJcUKEJZFg8pQ2jhAybK.json", "revision": 13}, "event": "project.datafile_updated"}'
```

Using the webhook secret token and the webhook payload as a string, your code should generate a compute signature of `sha1=b2493723c6ea6973fbda41573222c8ecb1c82666`, which can be verified against the webhook request header:

### Example webhook request header
```
X-Hub-Signature: sha1=b2493723c6ea6973fbda41573222c8ecb1c82666
Content-Type: application/json
User-Agent: AppEngine-Google; (+http://code.google.com/appengine; appid: s~optimizely-hrd)
```
[block:callout]
{
  "type": "success",
  "title": "Tip",
  "body": "Use a tool like Request Bin to test your webhooks. Request Bin lets you create a webhook URL to process HTTP requests and render the data in a human-readable format. Click *Create a RequestBin* on the [Request Bin](http://www.requestbin.com) site and use the resulting URL as your endpoint URL for testing."
}
[/block]