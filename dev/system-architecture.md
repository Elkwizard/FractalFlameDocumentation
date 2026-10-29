# System Architecture

> [!NOTE]
This page lays out the structure of [FractalFlame](https://xentempest.github.io/FractalFlame/FractalFlame.html)'s TypeScript implementation, but assumes familiarity with the software's purpose and algorithm. For information on the mathematical underpinnings, see [here](../usr/what-is-a-fractal-flame.md), and for new users, see [here](../usr/what-is-fractalflame.md).

## Overview
FractalFlame is implemented as a minimalist [TypeScript](https://www.typescriptlang.org/) project to be run directly either locally as file or as a locally- or remotely-hosted website. For information on setting up a valid development environment, see [here](./setting-up-fractalflame-locally.md).

## Files

FractalFlame's functionality is split into conceptually distinct files, which are globally included with HTML `<script>` tags. These files are as follows:

* `FractalFlame.ts` contains the bulk of the implementation, and coordinates the other classes to create a single global iterated function system. It also handles updating of the IFS, [histogram creation](./what-is-the-histogram.md), and rendering.
* `NonLinearFunctions.ts` defines a class, `NonLinearFunction`, which represents an NLF and handles its controls in the UI. This file also contains the list of all current NLF implementations. For a guide on how to add to this list, see [here](./adding-an-nlf.md).
* `VectorMath.ts` and `MatrixMath.ts` define classes for use in linear algebra. `Vector2D` represents a point in ℝ², and `Matrix2D` represents a 3x2 affine transformation matrix.
* `UI.ts` handles the majority of HTML event handling, with the notable exception of NLF parameter sliders.

## Architecture
> [!WARNING] FractalFlame is currently undergoing major internal reworkings. This section is delayed due to its impending irrelevance and will be added as soon as the dust settles.

## See Also
* [Setting up FractalFlame Locally](./setting-up-fractalflame.md)
* [Adding a Non-Linear Function](./adding-an-nlf.md)
* [Modifying the Log-Density Display](./modifying-the-log-density-display.md)
* [What is the Histogram?](./what-is-the-histogram.md)