---
title: "Example datafile"
slug: "example-datafile"
hidden: false
createdAt: "2019-08-21T19:32:16.460Z"
updatedAt: "2019-09-27T21:56:04.761Z"
---
The datafile is a JSON string that describes all the features running in a given environment and dependent entities like audiences and attributes. Normally, you won't need to interact with the datafile directly. The SDK exposes methods that operate on it internally.

The example below shows what a datafile might contain. Use it to understand the configuration information that is passed.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"accountId\": \"12345\",\n    \"anonymizeIP\": false,\n    \"botFiltering\": false,\n    \"projectId\": \"23456\",\n    \"revision\": \"6\",\n    \"version\": \"4\",\n    \"experiments\": [\n        {\n            \"key\": \"my_experiment\",\n            \"id\": \"45678\",\n            \"layerId\": \"34567\",\n            \"status\": \"Running\",\n            \"variations\": [\n                {\n                    \"id\": \"56789\",\n                    \"key\": \"control\",\n                    \"variables\": []\n                },\n                {\n                    \"id\": \"67890\",\n                    \"key\": \"treatment\",\n                    \"variables\": []\n                }\n            ],\n            \"trafficAllocation\": [\n                {\n                    \"entityId\": \"56789\",\n                    \"endOfRange\": 5000\n                },\n                {\n                    \"entityId\": \"67890\",\n                    \"endOfRange\": 10000\n                }\n            ],\n            \"audienceIds\": [],\n            \"forcedVariations\": {}\n        }\n    ],\n    \"featureFlags\": [\n        {\n            \"experimentIds\": [],\n            \"id\": \"56789\",\n            \"key\": \"price_filter\",\n            \"rolloutId\": \"67890\",\n            \"variables\": [\n                {\n                    \"defaultValue\": \"100\",\n                    \"id\": \"12345\",\n                    \"key\": \"min_price\",\n                    \"type\": \"integer\"\n                }\n            ]\n        }\n    ],\n    \"events\": [\n        {\n            \"experimentIds\": [\n                \"34567\"\n            ],\n            \"id\": \"56789\",\n            \"key\": \"my_conversion\"\n        }\n    ],\n    \"audiences\": [],\n    \"attributes\": [],\n    \"groups\": [],\n    \"rollouts\": [\n        {\n            \"experiments\": [\n                {\n                    \"audienceIds\": [],\n                    \"forcedVariations\": {},\n                    \"id\": \"23456\",\n                    \"key\": \"23456\",\n                    \"layerId\": \"34567\",\n                    \"status\": \"Running\",\n                    \"trafficAllocation\": [\n                        {\n                            \"endOfRange\": 5000,\n                            \"entityId\": \"45678\"\n                        }\n                    ],\n                    \"variations\": [\n                        {\n                            \"featureEnabled\": true,\n                            \"id\": \"56789\",\n                            \"key\": \"56789\",\n                            \"variables\": [\n                                {\n                                    \"id\": \"67890\",\n                                    \"value\": \"100\"\n                                }\n                            ]\n                        }\n                    ]\n                }\n            ],\n            \"id\": \"78901\"\n        }\n    ],\n    \"variables\": []\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]