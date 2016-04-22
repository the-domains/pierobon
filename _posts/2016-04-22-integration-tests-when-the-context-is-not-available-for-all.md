---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: Integration tests when the context is not available for all the required entities
datePublished: '2016-04-22T13:11:09.556Z'
dateModified: '2016-04-22T13:09:53.495Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
sourcePath: _posts/2016-04-22-integration-tests-when-the-context-is-not-available-for-all.md
published: true
url: integration-tests-when-the-context-is-not-available-for-all/index.html
_type: Article

---
Integration tests when the context is not available for all the required entities

Sometimes when you need to perform integration tests you have to create a full context in order to test your logic, but some entities might not be available in the current context, so you cannot add or modify them.

To solve this problem you can search for a related entity that match the condition you want to test (provided your test database has so much data you can assume an instance like the one you are looking for exists).

So, in the test you can create a record for the current context and then set the relationship to the records retrieved.

Then during the cleanup phase remember to remove the created record.