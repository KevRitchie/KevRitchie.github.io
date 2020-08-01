---
layout: post
title:  "Azure DevOps - Build Failed: project.assets.json missing"
date:   2020-08-01 00:19:42 +0100
categories: azure devops build pipeline agent pool
---

**The one thing you dread seeing...Azure DevOps [Build failed]**

For the last few hours I have been trying to sort a problem with a build that started failing out of the blue.  The solution compiled and ran locally with success and previous pipeline builds completed successfully.

<img src="/images/ProjectAssetsFail.png" alt="Azure DevOps Build Failed Message" />

The build was suddenly complaining about the project.assets.json file and that I had to run a NuGet package restore which would create the file for me.

I already had a NuGet Restore step in my pipeline, so what was the problem?  Why wasn't it restoring the files?

In an attempt to force the restore, I added a **/t:restore** parameter to the MSBuild task.  I could see that the NuGet feeds were being interogated and files downloaded and installed, but it would fail trying to restore a package from the Telerik NuGet feed.  Strange, as running a restore locally worked OK.

Checking one of the projects I could see that a Telerik package had an update available and it did relate to the project that was having the problem.  I upgraded the package and checked the solution back in to kick off the build pipeline.  It subsequently failed again with the same problem!

During my investigation, I noticed that our Hosted Agent was set to **VS2017**, which has since been deprecated.

Best to change that.

I changed the Agent Pool to **Azure Pipelines** and set the Agent Specification to **windows-2019**

<img src="/images/AzureDevOpsAgentPool.png" alt="Azure DevOps Agent Pool" />

You can see a list of the available virtual machine images here (Agent Specifications) [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#software). 

I also removed the **/t:restore** parameter from the MSBuild task that I previously added.  I was certain this wasn't the problem. 

Saving and queuing up a new build, I waited for the MSBuild task to complete.

Success!

I suspect the problem was down to the local NuGet package cache on the Virtual Agent needing a bit of a kick.

If anyone else comes across the problem, I hope this helps.

[Go to the Home Page]({{ '/' | absolute_url }})