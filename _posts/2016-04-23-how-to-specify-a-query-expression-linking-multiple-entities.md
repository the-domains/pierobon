---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: 'Sometimes you need to specify multiple Entities to join and the syntax for it might result a little bit cumbersome. '
datePublished: '2016-04-23T19:18:56.626Z'
dateModified: '2016-04-23T19:18:51.364Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
sourcePath: _posts/2016-04-23-how-to-specify-a-query-expression-linking-multiple-entities.md
published: true
url: how-to-specify-a-query-expression-linking-multiple-entities/index.html
_type: Article

---
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/2bad4ebb-bcc6-41c1-a888-33cc6cb884ad.jpg)

Sometimes you need to specify multiple Entities to join and the syntax for it might result a little bit cumbersome. 

You can join multiple entities by calling the method AddLink. 

You need to specify the Entity to retrieve in the QueryExpression constructor. 

If we would like to do the equivalent of the SQL code: 

SELECT 

entityColumnSet

FROM EntityA 

INNER JOIN EntityB 

ON EntityA.Field1 = EntityB.Field2 

we need to call the AddLink like this: 

AddLink(EntityB, Field1, Field2) 

public

EntityCollection

RetrieveServiceObjects(

Guid

salesOrderId,

ColumnSet

entityColumnSet)

{

QueryExpression

entityQuery =

new

QueryExpression

(

"custom\_serviceobject"

);

//define an order on the returned data

OrderExpression

oE =

new

OrderExpression

();

oE.AttributeName =

"custom\_order"

;

oE.OrderType =

OrderType

.Ascending;

entityQuery.Orders.Add(oE);

entityQuery.AddLink(

"custom\_action"

,

"custom\_serviceobjectid"

,

"custom\_serviceobject"

).

AddLink(

"salesorder"

,

"custom\_order"

,

"salesorderid"

).

LinkCriteria.AddCondition(

"salesorderid"

,

ConditionOperator

.Equal, salesOrderId);

entityQuery.ColumnSet = entityColumnSet;

entityQuery.Criteria =

new

FilterExpression

();

EntityCollection

retrievedEntities = Service.RetrieveMultiple(entityQuery);

return

retrievedEntities;

}

When we want to join Entities that have a N:N relationship it takes one step more:

The systemuser Entity has a N:N relationship with the role one. 

CRM created an intermediate Entity called systemuserroles that we need to use. 

Here we are trying to retrieve all the user that don't have a system administrator role 

QueryExpression teamQuery =

new

QueryExpression(

"systemuser"

);

ColumnSet teamColumnSet =

new

ColumnSet(

"fullname"

,

"systemuserid"

,

"internalemailaddress"

);

teamQuery.ColumnSet = teamColumnSet;

teamQuery.Criteria =

new

FilterExpression();

OrderExpression oETeamPhase1 =

new

OrderExpression();

oETeamPhase1.AttributeName =

"fullname"

;

oETeamPhase1.OrderType = OrderType.Ascending;

teamQuery.Orders.Add(oETeamPhase1);

teamQuery.Criteria.FilterOperator = LogicalOperator.And;

teamQuery.Criteria.AddCondition(

"isdisabled"

, ConditionOperator.Equal,

false

);

teamQuery.AddLink(

"systemuserroles"

,

"systemuserid"

,

"systemuserid"

).AddLink(

"role"

,

"roleid"

,

"roleid"

).

LinkCriteria.AddCondition(

"name"

, ConditionOperator.Equal,

"System Administrator"

);

References:

QueryExpression.AddLink Method (Overloaded): 

[http://msdn.microsoft.com/en-us/library/bb890678.aspx][0]

[0]: http://msdn.microsoft.com/en-us/library/bb890678.aspx