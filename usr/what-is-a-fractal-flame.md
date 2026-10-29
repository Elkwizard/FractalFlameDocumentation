# What is a Fractal Flame?

> [!WARNING] This explanation may be too technical for readers without experience in linear algebra, probability theory, or general mathematics.
For information on how these concepts can be used in the FractalFlame interface, see [here](./what-is-fractalflame.md).

## Overview

Fractal flames, the images created by [FractalFlame](https://xentempest.github.io/FractalFlame/FractalFlame.html), are records of a shockingly simple mathematical process called an iterated function system, or IFS. This is pretty much exactly what it sounds like. An iterated function system is a set of functions, and to visualize it, the functions are repeatedly applied to a set of points until the distribution of points converges. The distribution can then be made into an image by basing pixel brightness on the density of points in a given location, in a manner similar to a 2D histogram.

## Iterated Function Systems in Theory
For the purposes of this documentation, an IFS is a set *F* of functions from points to points (*F* ⊆ ℝ² → ℝ²). To create the visualalization, or flame, or the system, we take a set of points *P₀* uniformly distributed in [-1,&nbsp;1]². To iterate the system, we use the following update rule:

*Pₙ₊₁* = { *f* (*p*) | *p* ∈ *Pₙ*, *f* ∈ *F* }

Intuitively, each point in the set is replaced with the results of it being transformed by each of the functions in the IFS. After some satisfactory number of iterations *k*, the distribution of points can be observed. By mapping the [-1,&nbsp;1]² range to the screen, each pixel corresponds to some small square of the domain in which the points reside. By counting the number of points from *Pₖ* in each pixel and comparing to the overall maximum, a relative density can be computed for each pixel. That can then be converted to brightness, yielding a visualization of the distribution of *Pₖ*, which serves as a proxy for the IFS, *F*.

## Iterated Function Systems in Practice
One problem with executing the aforementioned procedure in practice is that the number of points in *Pₙ* grows by a factor of |*F*| with each step, rendering it infeasible for multi-function systems. To solve this, FractalFlame (based on the original [2003 paper by Scott Draves](https://flam3.com/flame_draves.pdf)) uses a stochastic alternative. In this version, instead of putting *all* function results into the next set of points, a given point instead *randomly chooses* one function from *F* to transform with. This guarantees that *Pₙ* doesn't grow, and has been shown to lead to equivalent results, given enough convergence time. This is the heart of FractalFlame's computation, and the method by which it computes images.

## Iterated Function Systems in FractalFlame
FractalFlame has some additional features beyond a basic IFS, most notably colors and Weights. From a mathematical perspective, both of these constitute only small additions to the algorithm, though they massively improve the resulting visuals.

**Weights** are simply real numbers associated with each function in *F*. By normalizing their sum to 1, they act as probabilities associated with each function. Since the stochastic method requires randomly choosing a function for each point, these probabilities bias those chances in favor of functions with higher weights.

**Colors** are 3D vectors (red, green, blue) on [0,&nbsp;1]³ associated with each funtion in *F*. In order to guarantee that the colors are both smooth and reflect the structure of the flame, the points in *Pₙ* become pairs of a point and a color. Whenever a point's position is updated by a given function, a small amount of the function's color is simultaneously blended into the point's color. Then, when the final image is being created, instead of counting the points in each pixel, their colors are summed (and normalized across the image) to directly produce the final color.

> [!NOTE] There are additional details to the exact way FractalFlame creates its images, but these are beyond the scope of the mathematical premise.
For more technical details, see the [architectural overview](../dev/system-architecture.md) and the [code on GitHub](https://github.com/XenTempest/FractalFlame/).