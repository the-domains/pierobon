---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: "CRM Plugins need to extend\_IPlugin, but the general convention is to create a class per Plugin. This leads to Plugin solutions that are really sparse and can lead to making conflicting change."
datePublished: '2016-04-28T18:17:24.989Z'
dateModified: '2016-04-28T18:17:05.550Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
sourcePath: _posts/2016-04-28-single-entry-point-for-all-the-plugins-for-an-entity.md
published: true
url: single-entry-point-for-all-the-plugins-for-an-entity/index.html
_type: Article

---
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/8a5329fd-bfd6-441b-81af-017fd4b82c7e.jpg)

CRM Plugins need to extend IPlugin, but the general convention is to create a class per Plugin. This leads to Plugin solutions that are really sparse and can lead to making conflicting change.

By experience it's much easier to have a single entry point for an Entity, then to call the required methods from there. 

Initially we usually keep the methods as functions inside the code, but as the complexity grows we refactor them out inside one or more BL class(es).

This is a simple example on how you can detect which operation triggered the operation, in which context the plugin will run (Pre or Post operation) and whether the execution will be sync or async.

using System;

using System.Collections.Generic; 

using System.Linq; 

using System.ServiceModel.Description; 

using System.Text; 

using System.Threading.Tasks; 

using System; 

using System.Linq; 

using Crm.Plugins.DataFix.Classes; 

using Microsoft.Xrm.Sdk; 

using Microsoft.Xrm.Sdk.Messages; 

using Microsoft.Xrm.Sdk.Query; 

namespace Crm.Plugins.DataFix 

{ 

public class Account : IPlugin 

{ 

private const int PreValidation = 10; 

private const int PreOperation = 20; 

private const int PostOperation = 40; 

private const int Synchronous = 0; 

private const int Asynchronous = 1; 

/// 

private readonly string \_validEntityLogicalName = "account"; 

private readonly Type\[\] \_allowedTypes = { typeof(Entity), typeof(EntityReference) }; 

public void Execute(IServiceProvider serviceProvider) 

{ 

var context = (IPluginExecutionContext)serviceProvider.GetService(typeof(IPluginExecutionContext)); 

if (!context.InputParameters.Contains("Target") || !\_allowedTypes.Any()) return; 

//retrieve the parameters to be passed 

var serviceFactory = 

(IOrganizationServiceFactory)serviceProvider.GetService(typeof(IOrganizationServiceFactory)); 

var service = serviceFactory.CreateOrganizationService(context.UserId); 

var tracingService = (ITracingService)serviceProvider.GetService(typeof(ITracingService)); 

Entity entity; 

switch (context.MessageName) 

{ 

case "Update": 

entity = context.InputParameters\["Target"\] as Entity; 

if (entity != null && 

String.Equals(entity.ToEntityReference().LogicalName, \_validEntityLogicalName)) 

{ 

switch (context.Stage) 

{ 

case PreOperation: 

var ownedAccounts = RetrieveAllOwnedAccounts(context, service, entity); 

tracingService.Trace(String.Format("Retrieved {0} entities", ownedAccounts.Entities.Count)); 

//throw new InvalidPluginExecutionException(String.Format("Retrieved {0} entities", ownedAccounts.Entities.Count)); 

ExecuteUpdateMultiple(tracingService, service, ownedAccounts); 

break; 

case PostOperation: 

break; 

case PreValidation: 

break; 

default: 

throw new Exception(String.Format("Invalid context stage {0}", context.Stage)); 

} 

} 

break; 

case "Associate": 

break; 

case "Disassociate": 

break; 

default: 

throw new InvalidPluginExecutionException("No valid step"); 

} 

} 

private void ExecuteUpdateMultiple(ITracingService tracingService, IOrganizationService service, EntityCollection ownedAccounts) 

{ 

var requestWithNoResults = new ExecuteMultipleRequest() 

{ 

// Set the execution behavior to not continue after the first error is received 

// and to not return responses. 

Settings = new ExecuteMultipleSettings() 

{ 

ContinueOnError = true, 

ReturnResponses = true 

}, 

Requests = new OrganizationRequestCollection() 

}; 

ownedAccounts.Entities.ToList().ForEach(account =\> 

{ 

account.Attributes\["fax"\] = "1111110"; 

var updateRequest = new UpdateRequest { Target = account }; 

requestWithNoResults.Requests.Add(updateRequest); 

}); 

var multipleResponse = (ExecuteMultipleResponse)service.Execute(requestWithNoResults); 

// Display the results returned in the responses. 

foreach (var responseItem in multipleResponse.Responses) 

{ 

if (responseItem.Fault != null) 

{ 

tracingService.Trace("Request: " + requestWithNoResults.Requests\[responseItem.RequestIndex\] + " Index: " + 

responseItem.RequestIndex + " Fault: " + responseItem.Fault); 

throw new InvalidPluginExecutionException("Error code: " + responseItem.Fault.ErrorCode + " Details: " + 

responseItem.Fault.ErrorDetails + " Message: " + responseItem.Fault.Message); 

if (responseItem.Fault.InnerFault != null) 

{ 

tracingService.Trace("Error code: " + responseItem.Fault.InnerFault.ErrorCode + " Details: " + 

responseItem.Fault.InnerFault.ErrorDetails + " Message: " + responseItem.Fault.InnerFault.Message); 

} 

tracingService.Trace("There was an error"); 

throw new InvalidPluginExecutionException("Problems"); 

} 

} 

} 

private EntityCollection RetrieveAllOwnedAccounts(IPluginExecutionContext context, IOrganizationService service, Entity entity) 

{ 

var quExpr = new QueryExpression(entity.LogicalName); 

quExpr.Criteria.AddCondition("ownerid", ConditionOperator.Equal, context.UserId); 

return service.RetrieveMultiple(quExpr); 

} 

} 

}