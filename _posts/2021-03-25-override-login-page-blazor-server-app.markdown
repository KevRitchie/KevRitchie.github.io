---
layout: post
title:  "Override the Login Page in a Blazor Server App"
date:   2021-03-25 19:57:06 +0100
categories: blazor server identity
tag: Blazor
---

I'll admit this one had me stumped.

When you create a Blazor Server App and add authorisation out of the box, you get some really neat baked in pages e.g. Login, Registration, Log Out etc., that takes the pain out of you having to code the entire user registration, authentication and authorisation process.

This is a fantastic feature and saves a tonne of time.

However, I was caught out a little.  I wanted to customise the Login page and remove some of the layout, but I couldn't find it in the Identity/Accounts folder.  How do we fix that?

If you right click on your project and choose New -> Add Scaffolded Item, then choose Identity from the menu on the left.

<img src="/images/AddNewScaffoldedItem.jpg" alt="Adding a new Scaffolded Item to your Blazor Server App" />

You'll get a list of pages you can override in the solution.

<img src="/images/AddIdentity.jpg" alt="Overriding Identity Pages in your Blazor Server App" />

Here you can choose to overwrite them all or just choose the one you're interested in.

You'll then be able to customise any of the pages you choose.

Be carfeul though, if you choose to override the Log Out page, you will run into some issues.  A resolution to this can be found [here](https://github.com/dotnet/aspnetcore/issues/17839){:target="_blank"}.

Need to know more about Blazor authentication and authorisation?  Check out this [page](https://docs.microsoft.com/en-us/aspnet/core/blazor/security/?view=aspnetcore-5.0){:target="_blank"} over on Microsoft Docs.

[Go to the Home Page]({{ '/' | absolute_url }})