---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: ''
datePublished: '2016-04-26T18:27:44.323Z'
dateModified: '2016-04-26T18:25:20.763Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
sourcePath: _posts/2016-04-26-visual-studio-package-manager-useful-commands.md
published: true
url: visual-studio-package-manager-useful-commands/index.html
_type: Article

---
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/a6977a3c-17b5-4633-a987-07c7f085da05.jpg)

**  
List installed packages for a given project:**

Get-Package -project projectName

**Update package to a specific version:**

Update-Package packageName -Version Major.Minor.Build

**Reinstall installed package:**

Update-Package -reinstall packageName

**Install package in a specific project:**

Get-Project projectName | Install-Package packageName

**Reinstall package in a specific project:**

Get-Project projectName | Update-Package --reinstall packageName