---
title: "Adding Math Formulas to a Hugo-based site"
description: ""
author: "Brent Stewart"
date: "2022-08-17T10:40:54-04:00"
draft: false
math: true
Victor_Hugo: "true"
picture: "hugo"
Focus_Keyword: "Math with Hugo"
youtube: ""
github: "https://github.com/brentstewart/nextpertise"
refs: ["https://mertbakir.gitlab.io/hugo/math-typesetting-in-hugo/","https://katex.org/docs/browser.html","https://www.overleaf.com/learn/latex/Mathematical_expressions"]
tags:
  - "hugo"
---

An upcoming article features some basic math, but rendering it in markdown is unaesthetic.  What you get is _E=mc^2_ when what you want is $ E=mc^2$.  My search led me to [KaTeX](https://katex.org/) which is a JavaScript library that let's you put $\LaTeX$ code into an HTML document.  LaTeX was created to typeset scientific papers, so it is built for displaying things like matrices and integrals.  I don't plan to publish math that intense, but LaTeX can help clearly display even a simple division equation and improve the readability of the post.  The best source of information I found was [Mert Bakir's blog](https://mertbakir.gitlab.io/hugo/math-typesetting-in-hugo/) and my usage is based on his work.

There are three steps to incorporating KaTeX with Hugo.
1. Create a partial template.  I added this to my theme by creating a file at _themes/next/layouts/partials/katex.html_, but it could also be added to the site at _layouts/partials/_ (note that the name _next_ is my theme name, so your's will differ).  Pull the code from the [KaTeX site](https://katex.org/docs/browser.html) by copying everything within the <head> tags.  You'll notice there's also a script in the code below - add that to the file as well.  Here is the file that is current in August, 2022, for this site.

```
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.css" integrity="sha384-Xi8rHCmBmhbuyyhbI88391ZKP2dmfnOl4rT9ZfRI7mLTdk1wblIUnrIq35nqwEvC" crossorigin="anonymous">

  <!-- The loading of KaTeX is deferred to speed up page rendering -->
  < script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.js" integrity="sha384-X/XCfMm41VSsqRNQgDerQczD69XqmjOOOwYQvr/uuC+j4OPoNhVgjdGFwhvN02Ja" crossorigin="anonymous"></script>

  <!-- To automatically render math in text elements, include the auto-render extension: -->
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/contrib/auto-render.min.js" integrity="sha384-+XBljXPPiv+OzfbB3cVmLHf4hdUFHlWNZN5spNQ7rmHTXpd7WvJum6fIACpNNfIR" crossorigin="anonymous"
    onload="renderMathInElement(document.body);"></script>

  <script>
      document.addEventListener("DOMContentLoaded", function() {
          renderMathInElement(document.body, {
              delimiters: [
                  {left: "$$", right: "$$", display: true},
                  {left: "$", right: "$", display: false}
              ]
          });
      });
  </script>
```

2. Include the partial file in all the pages where you want to use LaTeX.  Again, I chose to include this in my theme as part of the _/theme/next/layouts/partials/header.html_ file.  It can be included anywhere as long as Hugo builds the code outside the <body> tags.  This Hugo function looks for the presence of a parameter named _math_ which is set to True.  This keeps from loading KaTeX on pages where it is unnecessary.
```
  {{ if .Params.math }}{{ partial "katex.html" . }}{{ end }}
```

3. Finally, edit the default archetype file (themes/next/archetypes/default.md).  Changing the markdown engine is not required, but issues with KaTeX have been reported using the default Goldmark (I didn't encounter any issues with either in my testing).  I have the math parameter present but set to false, which will not load KaTeX javascript (similar to what would happen if I omitted the parameter).  I am including the parameter as a reminder to my future self.
```
  
  math: false
```

Use KaTeX in markdown by using two dollar signs ($$) as before and after delimiters for a standalone centered equation or single dollar signs for in-line equations.
You can find a good LaTeX resource at the [Overleaf](https://www.overleaf.com/learn/latex/Mathematical_expressions) site.

The last problem I had was finding some good examples!  So, here are a few equations to give you a feel for what is possible.

$\LaTeX$  

Bracket the equation with two dollar signs to center ($$) - x = {-b \pm \sqrt{b^2-4ac} \over 2a}

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$$

Use one dollar sign on each side to include in line.  For instance, acceleration a={\deltav}{t} renders $a= {\Delta v \over t}$

Finally, just to show the range of LaTeX, is Schrodinger's Equation - i \hbar \frac{\partial}{\partial t}\Psi(\mathbf{r},t) = \hat H \Psi(\mathbf{r},t).
$$i \hbar \frac{\partial}{\partial t}\Psi(\mathbf{r},t) = \hat H \Psi(\mathbf{r},t)$$