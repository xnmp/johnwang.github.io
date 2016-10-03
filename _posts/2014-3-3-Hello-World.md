---
layout: post
title: You're up and running!
---

Next you can update your site name, avatar and other options using the _config.yml file in the root of your repository (shown below).

![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.

First post yayayaya

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

$$\sum a_b$$

inline integral: $\int_a^b f(x) dx$ that was an inline integral

\(hjhppp\)

 {% raw %}
  $$a^2 + b^2 = c^2$$ --> note that all equations between these tags will not need escaping! 
 {% endraw %}



This is basically saying find $w$ and \textbf{$b$ }such that $w\cdot x_{i}+b\geq1$
if $y_{i}=1$, and $w\cdot x_{i}+b\leq-1$ if $y_{i}=-1$, and such
that $\left\Vert w\right\Vert $ is minimized. Ie, minimize $\left\Vert w\right\Vert $
such that 
\[
\mbox{sgn}\left(w\cdot x_{i}+b\right)=y_{i}
\]
for all $i$.

The point is that getting $n$ and $\tilde{b}$ is equivalent to specifying
a hyperplane. The margin is then 
\[
\frac{1}{\left\Vert w\right\Vert }=\min_{i}\left(n\cdot x_{i}-\tilde{b}\right)
\]
The good part about $w$ is that we can get rid of more unknowns in
the constraint. 

