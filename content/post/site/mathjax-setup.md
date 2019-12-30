---
title: MathJax Setup in Hugo
date: 2019-08-19
comments: true
draft: false
disableToc: false
tag: ["site"]
---

With the intention to share my learnings related to operations research, I have to find a way to render mathematical formulas inside my posts.
Luckily, there already exist some excellent approaches for this purpose and MathJax is one of the best among them.
I used MathJax before with the previous theme *mainroad*, but couldn't remember how it was configured in the first place.
With a bit duplicate work of digging online, I found the original solution and wanted to share it here in case I forgot it some days later.

### Credits to the Original Post

The solution was posted [here](https://pennbay.github.io/tech/mathjax.with.hugo/) (with link to the original post shown below). **All credits to this approach should be given to the original author.**

```
https://pennbay.github.io/tech/mathjax.with.hugo/
```

### Solution

Below shows all the directories and files within the root directory of this site.
In my case, the root directory is *~/dev/blog/klian-site*:
```
total 16
drwxr-xr-x   3 klian  74715970    96B Aug 17 13:03 archetypes
-rw-r--r--   1 klian  74715970   1.5K Aug 19 20:27 config.toml
drwxr-xr-x   4 klian  74715970   128B Aug 19 08:29 content
drwxr-xr-x   2 klian  74715970    64B Aug 17 14:26 data
-rwxrwxrwx   1 klian  74715970   537B Aug 17 15:05 deploy.sh
drwxr-xr-x   5 klian  74715970   160B Aug 17 13:17 layouts
drwxr-xr-x  27 klian  74715970   864B Aug 30  1754 public
drwxr-xr-x   3 klian  74715970    96B Aug 17 13:05 resources
drwxr-xr-x   4 klian  74715970   128B Aug 17 13:11 static
drwxr-xr-x   3 klian  74715970    96B Aug 17 13:04 themes
```

Create a *partials* directory within the *layouts* directory:

```
 ~/dev/blog/klian-site/layouts ll
total 0
drwxr-xr-x  3 klian  74715970    96B Aug 17 13:17 _default
drwxr-xr-x  4 klian  74715970   128B Aug 19 20:31 partials
drwxr-xr-x  3 klian  74715970    96B Aug 17 13:17 shortcodes
```

Then create a file *mathjax_support.html* with content below within the *partials* directory:

```
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre','code'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>
```

The last thing to do is to add the *mathjax_support.html* to the header file:

```
<link rel="stylesheet" href="/css/tweaks.css"/>

{{ partial "mathjax_support.html" . }}
```

My partials directory now looks like this:

```
 ~/dev/blog/klian-site/layouts/partials ll
total 16
-rw-r--r--  1 klian  74715970    87B Aug 19 20:31 head_custom.html
-rw-r--r--  1 klian  74715970   553B Aug 19 20:30 mathjax_support.html
```

We've successfully setup MathJax now! 

### Usage

Now let's play with some math symbols.
To do inline math, the code below

```
I am an inline variable $x_i$.
```

produces *I am an inline variable $x_i$*.

For display mode, use double $ mark:

```
$$
\begin{align}
\text{min.} \quad & \sum_{i = 0}^{\infty}x_i \newline
\text{s.t.} \quad & x_i \in {0, 1}
\end{align}
$$
```

renders
$$
\begin{align}
\text{min.} \quad & \sum_{i = 0}^{\infty}x_i \newline
\text{s.t.} \quad & x_i \in {0, 1}
\end{align}
$$.

### Reference
1. Setting MathJax with Hugo, https://divadnojnarg.github.io/blog/mathjax/