---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: 'TO DO LIST FOR AN EFFECTIVE MANAGED SOLUTION DEPLOYMENT IN DYNAMICS CRM '
datePublished: '2016-05-05T17:22:58.439Z'
dateModified: '2016-05-05T17:22:51.294Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
sourcePath: _posts/2016-05-05-to-do-list-for-an-effective-managed-solution-deployment-in-d.md
url: to-do-list-for-an-effective-managed-solution-deployment-in-d/index.html
_type: Article

---
**TO DO LIST FOR AN EFFECTIVE MANAGED SOLUTION DEPLOYMENT IN DYNAMICS CRM**
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/7b6b4ec6-5360-4178-bb97-1d2a14f992e2.jpg)

It is quite difficult to remember every single step when deploying a managed solution in another environment. Many times there are simply too many variables into play.

For this reason I have been tracking the operation to perform, updating the list when something new came out.

Here's a list of what I do when I deploy a managed solution. It is far from comprehensive but it feels good to have some guideline when you have to face all the things that might go wrong during a deployment.

1\. Check which duplicate detection rules are active

2\. Deselect the activate steps and workflows after import

3\. Take a screenshot of all the warning generated during the import

4\. Open the steps in the default solution and activate them

5\. Open every workflow and check that the references to every record (the ones with an ID) is updated to the correspondent version in the new environment

6\. Activate every workflow

7\. Create sample data in order to start the workflows once. Contingent mails should be sent to a test mail address, not to a customer mail address

8\. Open the forms of the entities in the solution, test the creation of a new record. Then delete the new record

9\. Correct the reference to the new environment for all the files outside CRM (webpages,...) if needed

10\. Reactivate the duplicate detection rules that were active before the import and have been inactivated by the import

And you? Do you keep such a list? Do you have additional steps that you want to share?