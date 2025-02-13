---
date: 2024-03-20T22:12:00.000+01:00
description: ''
published: true
slug: 2024-20-blog-migration
tags:
- legacy-blogger
readtime: 1
title: Migrating blog from blogger.com to mkdocs and GitHub pages
---
## Why?
Simple. The management web interface was a bit painful in the beginning and it is even more so in 2024. It was simply time to move to something like Git, static page generator and CI/CD for deployment.

## How

I found this great blogpost [Daniel Roy Greenfeld - Blogger to Markdown Script](https://daniel.feldroy.com/posts/2022-02-blogger-to-markdown-script) . By using the script and the described method it was easy to convert old posts from blogger.com to MD. Though the images need manual download and linking to the primary source. Or I may try to find the originals to see if they are better quality. But this remains TODO for now.

Then I just created quick GitHub actions to deploy the blog based on this article [Material for MkDocs - Publishing your site](https://squidfunk.github.io/mkdocs-material/publishing-your-site/).

The only downside is that the web URL is bound to be `<username>.github.io/<reponame>` but I guess it is a sensible simplification.