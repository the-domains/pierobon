---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: 'namespace WebApplication1 '
datePublished: '2016-04-26T18:25:31.407Z'
dateModified: '2016-04-22T20:22:43.398Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
sourcePath: _posts/2016-04-26-namespace-webapplication1.md
published: true
url: namespace-webapplication1/index.html
_type: Article

---
namespace WebApplication1 

{ 

public partial class WebForm2 : System.Web.UI.Page 

{ protected void Page\_Load(object sender, EventArgs e) { if (!IsPostBack) { Session\["PageRefresh"\] = System.DateTime.Now.ToString(); } } protected void Page\_PreRender(object sender, EventArgs e) { stateView = DateTime.Parse(Session\["PageRefresh"\].ToString()); } protected void Button1\_Click(object sender, EventArgs e) { if (DateTime.Parse(Session\["PageRefresh"\].ToString()) != stateView) { Label1.Text = "Refreshed"; Session\["PageRefresh"\] = System.DateTime.Now.ToString(); } else { Label1.Text = "not refreshed"; } } } }