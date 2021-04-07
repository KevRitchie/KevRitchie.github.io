---
layout: post
title:  "Setting your ASP.NET Core Environment on Deployment (Right Click Publish) 🐱‍🏍"
date:   2021-04-07 14:41:06 +0100
categories: aspnet core environment
tag: ASP.NET Core
---

So you've "Right Click Published" your ASP.NET Core application and things are going great.  Suddenly you hit an error on the site with a message similar to this:

**The Development environment shouldn't be enabled for deployed applications. It can result in displaying sensitive information from exceptions to end users. For local debugging, enable the Development environment by setting the ASPNETCORE_ENVIRONMENT environment variable to Development and restarting the app.**

You've double checked the publish profile settings and everything looks good; you can see it's in release mode.  What gives?!

Well, when the host is built, it remembers the last environment setting it read.  In the case of your local development, it'll be reading from the launchSettings.json file and thus would have been set to **Development**.

**How do we fix that for Deployment?**

There are a number of ways to fix this on deployment and I've included some useful links below to provide more depth on the subject and the many ways you can deploy your application.

In our case, we're using "Right Click Publish" method from within Visual Studio.

To set the `ASPNETCORE_ENVIRONMENT` variable on deployment, we need to edit the Publish Profile (.pubxml) file found under the Properties->PublishProfiles folder of your project.  For our example, this is the `IISProfile.pubxml` file.

All we need to do is add the `<EnvironmentName>` property to the file and set it to Production.  There will be other properties in the file, you can add this property as the last item in the `PropertyGroup`.

```
<PropertyGroup>
  <EnvironmentName>Production</EnvironmentName>
</PropertyGroup>
```
*Out of the box you can set the property to Development, Staging or Production - you can even add custom ones.  More details can be found in the useful links below.*

Don't forget to save!

When you next Publish your application the ASPNETCORE_ENVIRONMENT variable will be added to the web.config file and set to Production.

You should see something like this:

```
<environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Production" />
</environmentVariables>
```

**Useful Links**

The Publish Profile environment documentation can be found [here](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles?view=aspnetcore-3.1#set-the-environment).

For other platforms (including Azure), additional information on Environments and setting the Environment variable can be found [here](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments?view=aspnetcore-3.1#environments).

[Go to the Home Page]({{ '/' | absolute_url }})