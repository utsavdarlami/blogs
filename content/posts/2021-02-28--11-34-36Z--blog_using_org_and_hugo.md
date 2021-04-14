+++
title = "blog using org and hugo"
author = ["felladog"]
date = 2021-02-28T17:19:00+05:45
tags = ["blog", "org-mode", "org-roam", "hugo"]
categories = ["workflow"]
draft = false
+++

---

-   References :
    -   <https://ddavis.io/>  Copied shamelessly from here\*
    -   <https://seds.nl/>  **My ultimate plan**

-   Questions :

---

I have made a base site using [hugo](https://gohugo.io/) static generator.
The posts are written using org-mode from emacs. The plugin \`[ox-hugo](https://ox-hugo.scripter.co/)\` converts the org file to markdown and places it to the posts folder of the hugo site folder.
I have also used roam capture template so that the placeholder for tags and categories are predefined in new org files.
The placeholder includes roam\_tags, hugo\_tags, hugo\_categories, HUGO\_BASE\_DIR path


## Things i need to add {#things-i-need-to-add}

-   There is problem in site rendering when a org file is linked to a org file which has not been exported to markdown. I might not export all my notes to markdown because they might not be publishable.
-   Comment or link for creating issue in my blogs repo, **Added link to create issues**
-   Add biblography


## My ulitmate plan {#my-ulitmate-plan}

-   <https://seds.nl/>
-   has biblography
-   table of content in right, **I have Added it but need to make fix the design**