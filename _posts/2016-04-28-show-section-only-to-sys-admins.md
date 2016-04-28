---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: 'Ever wanted to show fields in a form only to SysAdmins, so that you do not need to define special forms or use Advanced Find?'
datePublished: '2016-04-28T18:34:27.156Z'
dateModified: '2016-04-28T18:33:57.399Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
sourcePath: _posts/2016-04-28-show-section-only-to-sys-admins.md
published: true
url: show-section-only-to-sys-admins/index.html
_type: Article

---
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/694dbdba-9b02-485b-8aec-18312f55b864.jpg)

Ever wanted to show fields in a form only to SysAdmins, so that you do not need to define special forms or use Advanced Find?

Here's a JS field that triggers the visibility of a section only to those users that have the Sys Admin role. The script can easily be adapted to other roles and functions.

function GuidsAreEqual(guid1, guid2) {

var isEqual = false; 

if (guid1 == null || guid2 == null) { 

isEqual = false; 

} 

else { 

isEqual = guid1.replace(/\[{}\]/g, "").toLowerCase() == guid2.replace(/\[{}\]/g, "").toLowerCase(); 

} 

return isEqual; 

} 

function UserHasRole(roleName) {

debugger; 

var serverUrl = Xrm.Page.context.getServerUrl(); 

var oDataEndpointUrl = serverUrl + "/XRMServices/2011/OrganizationData.svc/"; 

oDataEndpointUrl += "RoleSet?$top=1&$filter=Name eq '" + roleName + "'"; 

var service = GetRequestObject(); 

if (service != null) { 

service.open("GET", oDataEndpointUrl, false); 

service.setRequestHeader("X-Requested-Width", "XMLHttpRequest"); 

service.setRequestHeader("Accept", "application/json, text/javascript, \*/\*"); 

service.send(null); 

var requestResults = eval('(' + service.responseText + ')').d; 

// Change Started 

if (requestResults != null && requestResults.results.length == 1) { 

var role = requestResults.results\[0\]; 

// Change Ended 

var id = role.RoleId; 

var currentUserRoles = Xrm.Page.context.getUserRoles(); 

for (var i = 0; i < currentUserRoles.length; i++) { 

var userRole = currentUserRoles\[i\]; 

if (GuidsAreEqual(userRole, id)) { 

return true; 

} 

} 

} 

} 

return false; 

} 

function HideSectionIfNotAdmin(tabName, sectionName) {

if ((Xrm.Page.ui.tabs.get(tabName).sections.get(sectionName) === undefined) || (Xrm.Page.ui.tabs.get(tabName).sections.get(sectionName) === null)) { 

return; 

} 

var isUserSysAdmin = UserHasRole("System Administrator"); 

if (isUserSysAdmin) { 

Xrm.Page.ui.tabs.get(tabName).sections.get(sectionName).setVisible(true); 

} 

}