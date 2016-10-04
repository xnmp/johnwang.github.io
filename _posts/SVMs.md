---
layout: post
title: Support Vector Machines
---

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

%% LyX 2.1.4 created this file.  For more info, see http://www.lyx.org/.
%% Do not edit unless you really know what you are doing.
\documentclass[english]{article}
\usepackage{mathptmx}
\renewcommand{\familydefault}{\rmdefault}
\usepackage[T1]{fontenc}
\usepackage[latin9]{inputenc}
\usepackage{babel}
\begin{document}

\title{Support Vector Machines}

\maketitle

\section{Linear SVM}

We have a set of points $\left(x_{i},y_{i}\right)$ where each $x_{i}$
is a vector and each $y_{i}$ is either -1 or 1.


\subsection{Hard Margin}
\begin{itemize}
\item First consider the line $L=\left\{ y=mx\right\} $.
\item What is the distance from a point $p_{0}=\left(x_{0},y_{0}\right)$
to this line?
\item As we know from high school the answer is $p_{0}\cdot n$ where $n$
is the unit normal of the line. 
\item The equation $n\cdot p_{0}+\tilde{b}=0$ defines the set of points
lying on another line with gradient $m$, but with a different intercept. 
\item In fact, for the line (defined by) $y=mx+\tilde{b}$, then the points
lying on the line is defined by $\left(p_{0}-\tilde{b}\right)\cdot n=0$,
ie $p_{0}\cdot n-\tilde{b}\cdot n=0$, and we can denote $b=\tilde{b}\cdot n$. 
\end{itemize}

\subsubsection{The Optimization Problem}
\begin{itemize}
\item Equivalently this can be written 
\[
\frac{1}{\left\Vert w\right\Vert }\left(w\cdot p_{0}+\frac{b}{\left\Vert w\right\Vert }\right)=0
\]
We need to find $w$ and $b$ to maximize the ``margin'', in this
case $1/\left\Vert w\right\Vert $. 
\item This is basically saying find $w$ and $b$ such that $w\cdot x_{i}+b\geq1$
if $y_{i}=1$, and $w\cdot x_{i}+b\leq-1$ if $y_{i}=-1$, and such
that $\left\Vert w\right\Vert $ is minimized. 
\item Ie, minimize $\left\Vert w\right\Vert $ such that 
\[
\mbox{sgn}\left(w\cdot x_{i}+b\right)=y_{i}
\]
for all $i$.
\item The point is that getting $n$ and $\tilde{b}$ is equivalent to specifying
a hyperplane. The margin is then 
\[
\frac{1}{\left\Vert w\right\Vert }=\min_{i}\left(n\cdot x_{i}-\tilde{b}\right)
\]
The good part about $w$ is that we can get rid of more unknowns in
the constraint. 
\end{itemize}

\subsection{Soft Margin}
\begin{itemize}
\item If the classification is wrong, we need a measure of how wrong it
is.
\item This can be given by the hinge loss
\[
L\left(x,y\right)=\max\left(0,1-\left(w\cdot x+b\right)y\right)
\]
Now minimize the average hinge loss.
\item Remark: This is conceptually similar not entirely equivalent to maximizing
\[
\left(w\cdot x+b\right)y
\]
which is what was done at {[}1{]}. 
\end{itemize}

\subsubsection{Regularization}
\begin{itemize}
\item Have a parameter $\lambda$ that regulates the tradeoff between the
margin and the hinge loss.
\item Ie between getting more points right and getting wrong points less
wrong.
\item Now minimize
\[
\frac{1}{n}\sum_{i}^{n}L\left(x_{i},y_{i}\right)+\lambda\left\Vert w\right\Vert 
\]

\end{itemize}

\section{General Kernels}

What's actually happening here?
\begin{itemize}
\item We're basically picking a function $f$ so that the predictions are
given by 
\begin{eqnarray*}
\mbox{pred}\left(x\right) & = & \mbox{sgn}\left(f\left(x\right)+b\right)\\
f\left(x\right) & = & w\cdot x\\
 & = & \sum_{j}w_{j}x_{j}
\end{eqnarray*}

\item Of course, we pick $f$ to minimize the hinge loss
\begin{eqnarray*}
L\left(x_{i},y_{i}\right) & = & \max\left(1-y_{i}\left(f\left(x_{i}\right)+b\right),0\right)\\
\bar{L} & = & \frac{1}{N}\sum_{i=1}^{N}L\left(x_{i},y_{i}\right)
\end{eqnarray*}

\end{itemize}

\subsection{RBF Kernel}

This is where it gets more interesting: we can pick something else
for $f$
\[
f\left(x\right)=\sum_{i}^{n}w_{i}y_{i}\exp\left(-4\left\Vert x_{i}-x\right\Vert ^{2}\right)
\]

\begin{itemize}
\item Think of this as just superimposing a Gaussian hump on top of every
point in the training dataset, pointing up or down depending on $y_{i}$. 
\item Then to classify a point, just look at whether the humps sum to something
positive at that location. 
\end{itemize}

\subsection{General}

In fact we actually pick anything for $f$, as long as it's of the
form 
\[
f\left(x\right)=\sum_{i}\alpha_{i}y_{i}K\left(x_{i},x\right)
\]

\begin{itemize}
\item We require that $K$ be positive-semidefinite. (otherwise there might
be more than one solution). 
\item Without the positive semidefinite property all of these optimization
problems would be able to \textquotedblleft run to negative infinity\textquotedblright{}
or use negative terms {[}1{]}.
\item Remark: the $\alpha_{i}$ are positive, otherwise having $y_{i}$
would be redundant (it would also imply we could flip the sign of
$K$ with impunity, which is false). 
\end{itemize}

\subsection{Recasting the Linear Kernel}

To compare the above to the linear kernel, write 
\begin{eqnarray*}
K\left(x_{i},x\right) & = & \left\langle x_{i},x\right\rangle =\sum_{j}^{n}x_{ij}x_{j}\\
f\left(x\right) & = & \sum_{i}\alpha_{i}y_{i}\sum_{j}^{n}x_{ij}x_{j}\\
 & = & \sum_{j}^{n}\left(\sum_{i}\alpha_{i}y_{i}x_{ij}\right)x_{j}
\end{eqnarray*}
So that 
\begin{eqnarray*}
w_{j} & = & \sum_{i}\alpha_{i}y_{i}x_{ij}\\
w & = & \sum_{i}\alpha_{i}y_{i}x_{i}
\end{eqnarray*}

\begin{itemize}
\item Ie we have a linear combination of dot prducts which will reduce to
just a dot product in the end.
\item Graphically, think of it like this: the 3d plot for $x\cdot y$ is
just a tilted plane that passes through the origin. Superimposing
such planes results in just another such plane. Eventually you just
get a plane, and this is what you're testing is greater or less than
zero. 
\end{itemize}

\subsubsection{Support Vectors}

Are those for which $w_{i}\neq0$. In the linear case there are two
points $p_{1}$ and $p_{2}$ such that $p_{1}-p_{2}$ is parallel
to the dividing line. But this isn't necessarily the only way. 


\subsection{Higher Dimensional Projection}

Abstractly, the positive-definiteness can be captured by setting $K\left(x_{1},x_{2}\right)=\left\langle \phi\left(x_{1}\right),\phi\left(x_{2}\right)\right\rangle $
where $\phi:V\to W$ is some map between inner product spaces. 

For example
\[
\phi:\left(x,y\right)\mapsto\left(x,y,x^{2}+y^{2}\right)
\]
So that 
\begin{eqnarray*}
K\left(x,y\right) & = & \phi\left(x\right)\cdot\phi\left(y\right)\\
 & = & x\cdot y+\left(x_{1}^{2}+x_{2}^{2}\right)\left(y_{1}^{2}+y_{2}^{2}\right)
\end{eqnarray*}

\begin{itemize}
\item Note that this doesn't imply conjugate symmetry or sesquilinearity
at all, since $\phi$ can be anything, perhaps even discontinuous. 
\item The whole projection to a higher dimensional space metaphor/interpretation
really isn't all that useful. It's really trying to apply the intuition
of the linear kernel to general kernels, when the other way around
is much more natural. 
\end{itemize}

\subsection{Relationship to kNN}
\begin{itemize}
\item The SVM is actually the same as weighted kNN with $k$ infinite, except
that in this case $\alpha_{i}=1$ for all $i$. 
\item So an SVM is like a weighted kNN on steroids. 
\item Note that the rbf kernel is not separable, ie cannot be written in
the form 
\[
\exp\left(-4\left\Vert x_{i}-x\right\Vert ^{2}\right)=f\left(x_{i}\right)g\left(x\right)
\]

\end{itemize}

\subsection{Relationship to Logistic Regression}
\begin{itemize}
\item Remember that
\begin{eqnarray*}
\log\left(\frac{p}{1-p}\right) & = & \sum_{i}w_{i}x_{i}\\
 & = & w\cdot x
\end{eqnarray*}
and
\[
\mbox{pred}\left(x\right)=\mbox{sgn}\left(w\cdot x+\beta\right)
\]
where $\beta$ is some function of the probability threshold. 
\item So you're optimizing $w$ again, which is the vector of coefficients. 
\item What we have is a linear SVM where you're maximizing the log likelihood
instead of the margin (or minimizing the hinge loss). \end{itemize}
\begin{thebibliography}{1}
\bibitem{key-1}http://www.win-vector.com/blog/2011/10/kernel-methods-and-support-vector-machines-de-mystified/

\bibitem{key-2}\end{thebibliography}

\end{document}
