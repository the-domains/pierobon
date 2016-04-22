---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: Integration tests when the context is not available for all the required entities
datePublished: '2016-04-22T13:18:05.612Z'
dateModified: '2016-04-22T13:17:15.480Z'
title: ''
author: []
sourcePath: _posts/2016-04-22-integration-tests-when-the-context-is-not-available-for-all.md
published: true
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
url: integration-tests-when-the-context-is-not-available-for-all/index.html
_type: Article

---
**Integration tests when the context is not available for all the required entities**
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/b1f46649-c97a-48bb-8564-1256aa12684a.jpg)

Sometimes when you need to perform integration tests you have to create a full context in order to test your logic, but some entities might not be available in the current context, so you cannot add or modify them.

To solve this problem you can search for a related entity that match the condition you want to test (provided your test database has so much data you can assume an instance like the one you are looking for exists).

So, in the test you can create a record for the current context and then set the relationship to the records retrieved.

Then during the cleanup phase remember to remove the created record.