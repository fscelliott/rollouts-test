---
title: "Integrate with Jira"
slug: "integrations"
hidden: false
createdAt: "2019-08-21T19:32:15.834Z"
updatedAt: "2019-09-27T21:56:04.763Z"
---
Integrating [Optimizely Rollouts](doc:welcome) with Jira enables you to connect your workflows, so you have better visibility and an improved workflow for managing features under development. 

See the the status of your flags and rollouts in Jira, so that you can tell whether flags are on or off, or rolled out to a percentage of users. When someone else on your team turns on a feature for a percentage of your audience, you’ll automatically see that information on the Jira issue.
[block:api-header]
{
  "type": "basic",
  "title": "Setup"
}
[/block]
<a id="install">**1. Install the Optimizely integration**</a>

1. Log in to your Jira instance as an Administrator.
2. To install the [Optimizely for Jira app](https://marketplace.atlassian.com/apps/1219783/optimizely-for-jira), click the Admin dropdown and choose *Add-ons*.

<a id="configure">**2. Configure Optimizely**</a>

1. Log in to Optimizely as an Administrator or Project Owner.
2. Navigate to *Settings* > *Integrations,* and toggle the switch *On*. 
3. Click *Connect to Jira* to initiate the authentication process.
4. On the OAuth page in Jira, select the Jira tenant URL (e.g., `company.atlassian.net`).
5. In Optimizely, select the same Jira tenant URL you just authenticated and click *Save*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e15e871-integrate.png",
        "integrate.png",
        2628,
        1372,
        "#f6f7f9"
      ],
      "border": true
    }
  ]
}
[/block]
<a id="enable">**3. Enable the new Jira Issue view**</a>

Next, enable the new [Jira issue view](https://confluence.atlassian.com/jiracorecloud/the-new-jira-issue-view-938040503.html) to make the *Releases* section visible each of your Jira issues.

1. In Jira, click your profile avatar in the lower-left corner and select Personal settings.
2. Toggle the switch for the new Jira issue view.
[block:callout]
{
  "type": "info",
  "body": "If *Personal Settings* isn't listed as an option under your avatar, find the toggle switch under *Profile* instead.",
  "title": "Note"
}
[/block]
Congratulations! You've set up the integration between Optimizely and Jira. 

When linking Jira issues to Optimizely feature flags for the first time, each collaborator will be asked authenticate their Jira account.
[block:api-header]
{
  "title": "Link to a Jira issue"
}
[/block]
Here's how to link a feature flag to an issue in Jira.

1. Navigate to the *Features* dashboard and select your feature.
2. Click the *Actions* icon (**...**). 
3. From the dropdown, select *Connect to Jira*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8116082-link.png",
        "link.png",
        2426,
        642,
        "#f9f9fc"
      ],
      "border": true
    }
  ]
}
[/block]
4. Enter the Jira issue key in the search field and select *Add Issue*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/24a4c6e-jira-integration-add-issue.png",
        "jira-integration-add-issue.png",
        800,
        271,
        "#fcfbfb"
      ],
      "border": true,
      "sizing": "80"
    }
  ]
}
[/block]
Once you've selected all the Jira issues you'd like to link, click *Save and Link Issues*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7f5fac9-jira-integration-save-and-link-issues.png",
        "jira-integration-save-and-link-issues.png",
        750,
        368,
        "#f7f7fb"
      ],
      "border": true,
      "sizing": "80"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Unlink from a Jira issue"
}
[/block]
Here's how to unlink a feature flag from an issue in Jira:

1. Navigate to *Features* and select your feature.
2. Hover over the Jira ticket link. Select *Unlink*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ecf9862-unlink.png",
        "unlink.png",
        900,
        271,
        "#fafafd"
      ],
      "border": true
    }
  ]
}
[/block]
3. In the dialog, confirm by selecting *Unlink Jira Issue*. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/dffafc0-jira-unlink-jira-issue.png",
        "jira-unlink-jira-issue.png",
        521,
        221,
        "#faf0f1"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Troubleshoot"
}
[/block]
Here are some common issues and approaches to help you troubleshoot this integration.

<a id="verify">**How do I know if my integration works as expected?**</a>

To verify your integration is working as expected, navigate to *Settings* > *Integrations* and find the project integration settings for Jira. These should indicate that integration is switched on and connected with a Jira tenant URL.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/abc005c-jira-settings-integrations.png",
        "jira-settings-integrations.png",
        900,
        492,
        "#f8f9fa"
      ],
      "border": true
    }
  ]
}
[/block]
Alternatively, if you see the option to [link Jira issues](#section-section-link-to-a-jira-issue) or [unlink Jira issues](#section-unlink-from-a-jira-issue) to feature flags in the *Features* dashboard as shown in the sections above, your integration is working correctly.

If your integration isn't working as expected, make sure that your Jira Administrator has installed the [Optimizely for Jira app](https://marketplace.atlassian.com/apps/1219783/optimizely-for-jira) as an add-on before trying to authenticate from Optimizely. Optimizely must be installed as an add-on *before* any authentication attempts are made.

<a id="not-available">**My project settings show that Jira is enabled, but I can't link issues.**</a>

If your Integration settings show that the Jira integration is enabled, but you can't see an option to link a particular feature to an issue, you haven't authenticated your individual Jira account with Optimizely. On the Atlassian OAuth page, select the same Jira tenant URL as is listed in Optimizely's settings to fully authenticate as a user.

<a id="no-releases">**I don't see the Releases section in my Jira issues.**</a>

If you can't see the *Releases* section on the right side of your Jira issues, make sure that *New Jira Issue View* is switched on in your *Profile Settings* in Jira. If that doesn't work, check with your Jira Administrator that the [Optimizely for Jira app](https://marketplace.atlassian.com/apps/1219783/optimizely-for-jira) is installed in your Jira instance.