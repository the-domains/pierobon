---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: 'Sometimes when you need to perform integration tests you have to create a full context in order to test your logic, but some entities might not be available in the current context, so you cannot add or modify them.'
datePublished: '2016-04-22T20:23:49.290Z'
dateModified: '2016-04-22T19:57:23.365Z'
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
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/d73ea5de-f939-4088-aeab-88926f589b1e.jpg)

Sometimes when you need to perform integration tests you have to create a full context in order to test your logic, but some entities might not be available in the current context, so you cannot add or modify them.

To solve this problem you can search for a related entity that match the condition you want to test (provided your test database has so much data you can assume an instance like the one you are looking for exists).

So, in the test you can create a record for the current context and then set the relationship to the records retrieved.

Then during the cleanup phase remember to remove the created record.

\[TestClass\]

public class VerifyAccountLogicIsExecuted

{ 

protected int AccountId { get; private set; } 

protected bool Result { get; private set; } 

protected Account Account { get; private set; } 

protected string AccountNo = "0000001234569"; 

protected override void Arrange() 

{

if (Context.Accounts.Count(a =\> a.AccountNo.Equals(AccountNo)) == 1)

{ 

Context.Accounts.Remove(Account);

Context.SaveChanges(); 

} 

var Customer = ( from accounts in Context.Accounts

where account.Customer.CustomerServices.Any(cs =\> cs.Service.EnablePhoneSupport) 

select account.Customer).First();

Account = new Account() 

{ 

AccountId = 1987654321, 

Type = "TIC", 

Customer = Customer, 

AccountNo = AccountNo

}; 

Context.Accounts.Add(Account);

Context.SaveChanges(); 

AccountId = ( from account in Context.Accounts

where !account.Type.Equals( AccountType.Bank) && !!account.Type.Equals( AccountType.Insurance) &&

account.Customer.CustomerServices.Any(cs =\> cs.Service.EnablePhoneSupport)

select account.AccountId).First(); 

} 

protected override void Act() 

{ 

Result = Service.IsPhoneSupportEnabled(AccountId); 

} 

\[ IntegrationTest\] 

\[ TestMethod\] 

public void ThenResultShouldBeTrue() 

{ 

Assert.IsTrue(Result); 

} 

\[ TestCleanup\] 

protected override void CleanUp() 

{ 

if (Context.Accounts.Count(a =\> a.AccountNo.Equals(AccountNo)) == 1)

{ 

Context.Accounts.Remove(Account); 

Context.SaveChanges(); 

} 

} 

}