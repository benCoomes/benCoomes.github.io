---
layout: post
title:  "Hosting PDFs"
date:   2022-08-17 22:26:00 -0400
categories: jekyll
---

One of the requirements my friend has is that his site can host PDF files. This post shows how to do that with GitHub pages and Jekyll.

## Linking
It is easy to serve PDFs on a GitHub pages site. Add your PDF to your git repo, and then link to the relative path. For example, [this pdf](/assets/curveball.pdf) is hosted in my repo at `/assets/curveball.pdf` and linked in this post with the the markdown `[this pdf](/assets/curveball.pdf)`.

## Embedding
You can also embed the PDF directly into a post with HTML. This is the code used to render the PDF directly following.
```html
<object data="/assets/curveball.pdf" type="application/pdf" width="100%" height="350">
  PDF unable to render in this browser. Alt: <a href="/assets/curveball.pdf">curveball.pdf</a>
</object>
```
<object data="/assets/curveball.pdf" type="application/pdf" width="100%" height="350">
  PDF unable to render in this browser. Alt: <a href="/assets/curveball.pdf">curveball.pdf</a>
</object>

### Alternate Content
It is a good idea to include alternate content within the `<object>` element so there is a fallback in case the browser cannot render the pdf. Perhaps your site is viewed on a small screen, or you mispelled your PDF's name:
```html
<!-- curfball.pdf shouldn't exist, unless I forgot not to upload a file of the same name -->
<object data="/assets/curfball.pdf" type="application/pdf" width="100%" height="350">
  Unable to find 'curfball.pdf'. Did you mean <a href="/assets/curveball.pdf">curveball.pdf</a>?
</object>
```

<object data="/assets/curfball.pdf" type="application/pdf" width="100%" height="350">
  Unable to find 'curfball.pdf'. Did you mean <a href="/assets/curveball.pdf">curveball.pdf</a>?
</object>
