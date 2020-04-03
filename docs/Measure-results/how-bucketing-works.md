---
title: "How bucketing works"
slug: "how-bucketing-works"
hidden: false
createdAt: "2019-09-11T11:47:39.208Z"
updatedAt: "2019-09-12T19:07:50.343Z"
---
Bucketing is the process of assigning users to different variations of an experiment. The Full Stack SDKs evaluate user IDs and attributes to determine which variation they should see.

During bucketing, the SDKs rely on the [MurmurHash](https://en.wikipedia.org/wiki/MurmurHash) function to hash the user ID and experiment ID to an integer that maps to a bucket range, which represents a variation. MurmurHash is deterministic, so a user ID will always map to the same variation as long as the experiment conditions don’t change. This also means that any SDK will always output the same variation, as long as user IDs and user attributes are consistently shared between systems.

This operation is highly efficient because it occurs in memory, and there is no need to block on a request to an external service. It also permits bucketing across channels and multiple languages, as well as experimenting without strong network connectivity.
[block:api-header]
{
  "title": "Ensure consistent visitor bucketing"
}
[/block]
As mentioned above, the SDKs will only maintain consistent (“sticky”) variation assignments if the experiment conditions remain the same. Generally, adding variations or changing traffic will re-bucket existing users.

For example, imagine you are running an experiment with two variations (A and B), with an experiment traffic allocation of 40%, and a 50/50 distribution between the two variations. Optimizely will assign each user a number between 0 and 10000 to determine if they qualify for the experiment, and if so, which variation they will see. If they are in buckets 0 to 1999, they see variation A; if they are in buckets 2000 to 3999, they see variation B. If they are in buckets 4000 to 10000, they will not participate in the experiment at all. These bucket ranges are deterministic: if a user falls in bucket 1083, they will always be in bucket 1083.

In this example, if you change the experiment allocation to any percentage **except 0%**, Optimizely ensures that all variation bucket ranges are preserved whenever possible to ensure that users will not be re-bucketed into other variations. If you add variations and increase the overall traffic, Optimizely will try to put the new users into the new variation without re-bucketing existing users.

To continue the example, if you change the experiment allocation from 40% to 0%, Optimizely will not preserve your variation bucket ranges. After changing the experiment allocation to 0%, if you change it again, perhaps to 50%, Optimizely starts the assignment process for each user from scratch: Optimizely will not preserve the variation bucket ranges from the 40% setting.

To completely prevent variation reassignments, implement sticky bucketing with the User Profile Service, which uses a caching layer to persist user IDs to variation assignments.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/591c995-bucketing_1.png",
        "bucketing_1.png",
        700,
        258,
        "#eeeeec"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Forced variations and bucketing"
}
[/block]
If you assign a user to a variation using forced variation and ask the Optimizely API which variation the user is in, the Optimizely API returns the variation the user is forced into. This is true even if the user is not in the audience for the experiment: Optimizely returns the variation the user is forced into regardless of the audience conditions associated with the experiment.

[block:api-header]
{
  "title": "Bucketing IDs"
}
[/block]
Use the bucketing IDs feature to control variation assignments directly by using a different identifier to control how variations are computed. Although you cannot directly specify the bucket, supplying the same bucketing ID among users will ensure they map to the same bucket and are exposed to the same variation. This works by hashing the provided bucketing ID with the experiment ID.   

This can be useful for ensuring that many different users get a consistent experience. For example, you might want all users on the same team to get the same variation but still want to count them separately in your results.
[block:api-header]
{
  "title": "End-to-end workflow"
}
[/block]
Let’s walk through how the SDK evaluates a decision. This chart serves as a comprehensive example that explores all possible factors.
1. The Activate call is executed and the SDK begins its bucketing process.
2. The SDK ensures that the experiment is running.
3. It compares the user ID to the whitelisted user IDs. Whitelisting is used to force users into specific variations. If the user ID is found on the whitelist, it will be returned.
4. If provided, the SDK checks the User Profile Service implementation to determine whether a profile exists for this user ID. If it does, the variation is immediately returned and the evaluation process ends. Otherwise, proceed to step 5.
5. The SDK examines [audience conditions](https://help.optimizely.com/Target_Your_Visitors/Audiences%3A_Choose_which_visitors_to_include) based on the user attributes provided. If the user meets the criteria for inclusion in the target audience, the SDK will continue with the evaluation; otherwise, the user will no longer be considered eligible for the experiment.
6. The hashing function returns an integer value that maps to a bucket range. The ranges are based on the traffic allocation breakdowns set in the Optimizely dashboard, and each corresponds with a specific variation assignment.
7. If the bucketing ID feature is used, the SDK will hash the bucketing ID (instead of the user ID) with the experiment ID and return a variation.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a113486-bucketing_flowchart.png",
        "bucketing_flowchart.png",
        1004,
        547,
        "#f1f2f3"
      ]
    }
  ]
}
[/block]