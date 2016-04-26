---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: ' '
datePublished: '2016-04-26T18:31:06.190Z'
dateModified: '2016-04-26T18:31:00.444Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
sourcePath: _posts/2016-04-26-migrate-sugarcrm-to-another-instance.md
published: true
url: migrate-sugarcrm-to-another-instance/index.html
_type: Article

---
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/1dcf0d7d-e4fd-477a-9511-85c06b13e58d.jpg)

__

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