---
title: "Invite collaborators"
slug: "collaborators"
hidden: false
createdAt: "2019-08-21T19:32:15.823Z"
updatedAt: "2020-01-10T19:16:39.650Z"
---
Invite collaborators to your Rollouts account to allow your team to manage feature flags, roll features out, and roll them back. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7b70076-collaborators.png",
        "collaborators.png",
        2124,
        768,
        "#f8f9fb"
      ]
    }
  ]
}
[/block]
1. Navigate to *Settings* > *Collaborators*. 
2. Select *Add a Collaborator*. 
3. Enter the email addresses of your collaborators, and select their [permission](doc:collaborators#section-permissions) levels.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7048f55-invite.png",
        "invite.png",
        1262,
        548,
        "#fcfcfc"
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Permissions"
}
[/block]
Here's the matrix of permissions for each collaborator type. Each role includes the permissions for the one before it. To summarize:
- *Viewers* have read-only access and cannot modify features
- *Editors* can set up features in a local or staging environment, but can't rollout in production (you can customize this with [restricted environments](doc:manage-environments#section-security)
- *Publishers* can roll out features in production
- *Administrators* have full permissions to manage account settings
[block:parameters]
{
  "data": {
    "h-2": "Editor",
    "h-3": "Publisher",
    "h-4": "Administrator",
    "h-5": "Project Owner (Admin)",
    "h-1": "Viewer",
    "h-0": "Actions",
    "1-0": "Traffic allocation (edit)",
    "3-0": "Attributes (create, edit, archive)",
    "4-0": "Collaborators (invite, edit, delete)",
    "6-0": "Webhooks (create, delete)",
    "7-0": "Integrations (create, edit, delete)",
    "0-0": "Features (create, edit, archive)",
    "0-4": "**Yes** ",
    "1-4": "**Yes** ",
    "3-4": "**Yes** ",
    "4-4": "**Yes** ",
    "6-4": "**Yes** ",
    "7-4": "**Yes** ",
    "0-1": "No",
    "1-1": "No",
    "3-1": "No",
    "4-1": "No",
    "6-1": "No",
    "7-1": "No",
    "0-2": "*Restricted*",
    "1-2": "*Restricted*",
    "3-2": "**Yes** ",
    "4-2": "No",
    "4-3": "No",
    "6-3": "No",
    "7-3": "No",
    "3-3": "**Yes** ",
    "1-3": "**Yes** ",
    "0-3": "**Yes** ",
    "6-2": "No",
    "7-2": "No",
    "5-0": "Environments (create, edit, archive)",
    "5-1": "No",
    "5-2": "No",
    "5-3": "No",
    "5-4": "**Yes** ",
    "2-0": "Audiences (create, edit, archive)",
    "2-1": "No",
    "2-2": "**Yes** ",
    "2-3": "**Yes** ",
    "2-4": "**Yes** "
  },
  "cols": 5,
  "rows": 8
}
[/block]
** In restricted environments, Editors can create features but not roll them out. In [non-restricted environments](doc:manage-environments#section-security), they have full permissions to roll out features. We recommend using the Editor role to restrict access in a production environments, while allowing rollouts in a local environment or staging environment.