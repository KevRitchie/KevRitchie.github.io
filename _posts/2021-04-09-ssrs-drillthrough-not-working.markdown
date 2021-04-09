---
layout: post
title:  "Fixing a SSRS Report Drillthrough Issue"
date:   2021-04-09 15:48:06 +0100
categories: ssrs reporting
image: /images/SSRSLogo.png
tag: SSRS
---

It's not uncommon to have a SQL Server Reporting Services (SSRS) Report with a drillthrough report and for this particular project I've been working on, this was a requirement.

To give a bit of background.  The application itself is a Blazor Server App which includes an iFrame pointing to a ASP.NET Web Application hosting the SSRS Report Viewer control to serve up the reports.  The reason for this approach is that the SSRS Report Viewer control does not work with Blazor.

Everything was working great until we noticed that you couldn't drill through to other reports. Gah!

What could be the problem?

First thoughts were, could it be an issue with the iFrame?  A quick test running the report directly in the ASP.NET Web Application produced the same issue!  So it's not the iFrame.

Turns out the resolution is a simple one...

...wrap the code that renders the report in an `!IsPostback` in the page load of the hosting application.  Hey presto, drill through now works.

```
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        rvDashboardReports.ServerReport.ReportPath = "/";

        Uri uri = new Uri(Properties.Settings.Default.ReportServerURL);

        rvDashboardReports.ServerReport.ReportServerUrl = uri;
        rvDashboardReports.ServerReport.Refresh();
    }
}
```

Sometimes it's the easy fixes that can keep you scratching your head thinking, what have I done wrong?!