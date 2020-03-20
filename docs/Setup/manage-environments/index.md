---
title: "Manage environments"
slug: "manage-environments"
hidden: false
createdAt: "2019-08-21T19:32:15.837Z"
updatedAt: "2019-08-26T19:32:34.494Z"
---
# THIS IS MY TEST SYNC PAGE



I'm making all changes to this page in my local env in Typora and/or VSCode, NOT in the web ui at dash.readme!

sync tool:

https://github.com/flowcommerce/readme-sync

markdown repo:

https://github.com/fscelliott/rollouts-test



 (the markdown is a small subset of rollouts; just the 'setup' category)



Steps I took:

- exported the rollouts doc project
- chose a small subset to play with: 'setup'
- created v4.0 in the web ui
- restructured the dirs, renamed files according to the readme-sync tool conventions
- manually downloaded an image from the docs (since readme doesn't actually export the images)

(note to self did I run: 
curl https://dash.readme.io/api/v1 -X GET -u <key> at some point?)

- ran (in my case) npx ts-node sync/index.ts --apiKey <key> --version 4.0 --docs /mnt/c/Users/franc/Documents/GitHub/rollouts-test/docs/ 

..and it works!

**images experiments**

So the 1st thing I want to test is, how do I get images? (readme export doesn't actually export the images)

Well, I manually downloaded some images, stored them in my github repo, and then tried to reference them in a native ReadMe image block.

that was a no-go; seems those image blocks maybe only accept readme urls?



Anyway so then I just tried a 'normal' markdown image syntax and that works, yay!

Behold this image originally in readme, now saved at 

 https://raw.githubusercontent.com/fscelliott/rollouts-test/master/images/6f02eed-new_environment.png :  

1. ![test image](https://raw.githubusercontent.com/fscelliott/rollouts-test/master/images/6f02eed-new_environment.png) .

  

  **next steps/questions**: 

- how does  'suggest edits'  work with this?
- how does changes made in the web ui work with this?(I assume this tool would just overwrite whatever changes were made in the web ui) -- update, yup, I think the web UI gives me a merge conflict warning...
- how does syncing of deleted pages work? and new pages?  test:
  - I have a superset of docs in the web UI in 1 category, and only sync a subset of them. do the other docs in the web UI get deleted?
  - I have a superset of docs in Github in 1 category, and only a subset in the web UI. do the docs get added?
