---
title: "A new beginning"
url: "a-new-beginning"
date: 2021-06-28T13:54:46.0000000Z
lastmod: 2021-06-28T13:54:46.0000000Z
tags: ["Hugo","Azure Static Web App","Continuous Delivery","Continuos Integration","CI/CD"]
draft: false
resources:
- name: "featured-image"
  src: "featured-image.jpg"
---
Hi all! Welcome in the fourth version of my blog.

I have developed the first version almost 15 years ago moving my first steps into web development in **PHP**. 

After few years I moved to **ASP.NET** because along with my professional skills I have improved my knowledge of .NET Framework, so using ASP.NET was the natural result of this path.

Then, after additional few years, web has become more complex, with an increasing number of features required to be developed to move with the times (RSS feed, analytics integration, comment platform integration, and so on), so I started using a CMS, choosing Orchard CMS (simply because it was looking a promising project and was developed in .NET).

This fourth version has embraced a completely different approach. Instead of moving to a new complex and feature rich CMS, I have decided to use a static website generator called [HUGO](https://gohugo.io/). If you don't know it, take a look, I think it deserves it!

Basically the HTML and JS of this website is not produced by some server side code that generates web pages as output when a request arrives. All the HTML and JS are statically produced by HUGO as part of a fully automated CI/CD process :)

The overall process is the following:
- I create new contents in a format supported by HUGO (like MD, YAML, and so on) in my local git repository
- I push changes on my GitHub repository on https://github.com/vifani/personal-site
- A GitHub Action triggered on commit/PR merge builds my website (basically it executes the command "hugo") and deploys it on an Azure Static Web Apps
- The [Azure Static Web Apps](https://docs.microsoft.com/en-us/azure/static-web-apps/overview) serves the updated version of my blog website :smile:

The result is a blazing fast website (because no server side code executes) that is hosted on a very cheap, but powerful service like Azure Static Web Apps.

Static Web Apps is an amazing new Azure service that provides many interesting features:
- Web hosting for static content (suitable for SPA developed in Angular, React, Vue.JS, for WebAssembly applications developed in Blazor or also to work with frameworks like Hugo, Gatsby, VuePress)
- First-class GitHub and Azure DevOps integration to automate build and deployment processes
- **Free SSL certificates** (yes, you have seen right :wink:, take a look [here](https://docs.microsoft.com/en-us/azure/static-web-apps/custom-domain?tabs=azure-dns))
- Globally distributed static content

Oh, and yes, I decided to switch to English language for most of my new contents. Even though can be challenging to express non-technical topics in a different-from-native language, I think this is a very good opportunity for me to improve and learn.