---
layout: post
title: "Example content"
date: "2013-01-01"
slug: "example_content"
description: "Example content from lanyon. If page description is more than 140 words, it will be shown as post summary on home page and blog index else post excerpt will be shown. Same rule is for html meta description: >140 words in description or first 50 words of posts will be shown as summary. Page excerpt supports markdown formatted summary."
use_math: true
category: 
  - views
  - featured
# tags will also be used as html meta keywords.
tags:
  - examples
  - common_tag
show_meta: true
comments: true
mathjax: true
gistembed: true
published: true
noindex: false
nofollow: false
# hide QR code, permalink block while printing.
hide_printmsg: false
# show post summary or full post in RSS feed.
summaryfeed: false
## for twitter summary card with squared image and page description or page excerpt:
# imagesummary: foo.png
## for twitter card with large image:
# imagefeature: "http://img.youtube.com/vi/VEIrQUXm_hY/0.jpg"
## for twitter video card: (active for this page)
videofeature: "https://www.youtube.com/embed/iG9CE55wbtY"
imagefeature: "http://img.youtube.com/vi/iG9CE55wbtY/0.jpg"
videocredit: tedtalks
---

Howdy! This is an example blog post that shows features supported in **lanyon-plus** theme. See [raw post](https://raw.githubusercontent.com/dyndna/lanyon-plus/master/_posts/2013-01-01-example-content.md) for required YAML header and liquid tag specifications.

### MathJax

Let's test some inline math $x$, $y$, $x_1$, $y_1$.

This formula $f(x) = x^2$ is an example.

Now a inline math with special character: $\|\psi\rangle$, $x'$, $x^\*$ and $\|\psi_1\rangle = a\|0\rangle + b\|1\rangle$

Test a display math:
$$
   |\psi_1\rangle = a|0\rangle + b|1\rangle
$$
Is it O.K.?

Test a display math with equation number:
\begin{equation}
   |\psi_1\rangle = a|0\rangle + b|1\rangle
\end{equation}
Is it O.K.?

Test a display math with equation number:

$$
  \begin{align}
    |\psi_1\rangle &= a|0\rangle + b|1\rangle \\
    |\psi_2\rangle &= c|0\rangle + d|1\rangle
  \end{align}
$$
Is it O.K.?

And test a display math without equaltion number:
$$
  \begin{align*}
    |\psi_1\rangle &= a|0\rangle + b|1\rangle \\
    |\psi_2\rangle &= c|0\rangle + d|1\rangle
  \end{align*}
$$
Is it O.K.?

Test a display math with equation number:
$$
\begin{align}
    |\psi_1\rangle &= a|0\rangle + b|1\rangle \\
    |\psi_2\rangle &= c|0\rangle + d|1\rangle
\end{align}
$$
Is it O.K.?

And test a display math without equaltion number:
$$
\begin{align*}
    |\psi_1\rangle &= a|0\rangle + b|1\rangle \\
    |\psi_2\rangle &= c|0\rangle + d|1\rangle
\end{align*}
$$

### gist embed

Below is a partial code showing main steps of merge function.

<code data-gist-id="0fe211678316cc53370c" data-gist-file="merge_tables_datatable.R" data-gist-line="50-52,57,65-69,80,88-90,100-106"></code>

### References

Multiple[^1]<sup>,</sup>[^3] and with comments[^2]

Want to see something else added? <a href="https://github.com/dyndna/lanyon-plus/issues/new">Open an issue.</a>

[^1]: [lanyon theme](http://lanyon.getpoole.com)
[^2]: 
    [lanyon-plus theme](https://github.com/dyndna/lanyon-plus "accessed on {{ page.date | date: '%B %d, %Y' }}")

    >Excerpt: Sample post showing enabled features for post: `inline code`, `text highlight`{:.yelhglt}, code block with syntax highlights, embed gist, stylized blockquotes, video and image cards for twitter, markdown tables, inline and code bocks having mathjax support, references, print format, disqus comments, related posts, tags.

[^3]: [About]({{ site.url }}/about)
