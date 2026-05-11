---
layout: post
title:  "Blog Changes"
date:   2026-05-11 4:24:14 -0600
categories: Blog Changes
---

## Blog bug fixes
In the Gemfile of this blog, the GITHUB_PAGES_VERSION had reset to 'GITHUB_PAGES_VERSION', and needed to be changed to 232, as that is the current version of github pages. The error was encountered when attempting to launch a local jekyll server. This error has been resolved.

---

## Detailed Error Report

 The exact error message was:
``` 
in `parse': Illformed requirement ["~> GITHUB-PAGES-VERSION"] (Gem::Requirement::BadRequirementError)
raise BadRequirementError, "Illformed requirement [#{obj.inspect}]"
```

---

After reading through the additional syntax, I re-read the Gemfile in the main branch 'docs' for the blog website, in the repository 'Ldanderson3/ldanderson3.github.io/'. I noticed that 'GITHUB-PAGES-VERSION' mentioned above was not a variable, and needed to be changed to the current github pages version. After changing to '232', the error was resolved.

## Lessons learned

After this error, I learned that it is important to re-read syntax and documetation of new lanaguages. I am new to Jekyll and its related languages and packages, and having errors like this is expected. However going back to documentation will help prevent errors like this in the future.