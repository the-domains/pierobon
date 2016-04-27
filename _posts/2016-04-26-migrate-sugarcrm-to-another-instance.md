---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: ''
datePublished: '2016-04-27T05:16:12.871Z'
dateModified: '2016-04-27T05:16:12.200Z'
title: ''
author: []
sourcePath: _posts/2016-04-26-migrate-sugarcrm-to-another-instance.md
published: true
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
url: migrate-sugarcrm-to-another-instance/index.html
_type: Article

---
__
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/9dbef2e5-009f-44ea-916c-dae1da403598.jpg)

_Copy the regional settings:_

* Date settings
* Time settings
* Language settings
* Currency settings
* Password settings
* System settings
* Edit available languages

_Copy the workflows:_

* Create the same workflows
* Copy the alerts and steps

_Customisations:_

* Export the customisation from Sugar from the source env
* Import the package in the Module Loader in the destination env
* Copy the translations from custom/include/language/ from source to destination

_Additional modules:_

* Export the modules from Module Builder inthe source env
* Import the package in the Module Loader in the destination env

_Copy the panels layout:_

* Replicate the panels layout
* Replicate the subpanels layout

_Copy the security roles:_

* Create the same security roles
* Copy the same configuration for the security roles

_Copy the users:_

* Configure the mail setup
* Create the users
* Assign the same security roles to the users as in the source env
* Copy the reports
* Copy the dashboards