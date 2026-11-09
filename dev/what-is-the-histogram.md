# What is the Histogram?

> [!NOTE] Note
> This explanation assumes familiarity with the [system's architecture](./system-architecture.md) and the [mathematical basis](../usr/what-is-a-fractal-flame.md#weights-and-colors) of the rendering system.

The histogram is the core of [FractalFlame](https://xentempest.github.io/FractalFlame/FractalFlame.html)'s rendering, and is the way by which the colors of the functions in the IFS are converted into an image. As a refresher, when the points of the IFS are updated in each cycle, their colors are blended with those of the functions they are updated by. With this context, the histogram can be precisely defined as a screen-sized array of colors which additively accumulates the colors from each point in the IFS. In the code, the histogram is represented by the `histogram` variable, which is a 1D array of RGBA `Color` objects with length equal to the number of pixels in the screen.

## Histogram Creation
On each update cycle of the IFS, after a point is updated, its color is directly added component-wise to the histogram element associated with its position. Since all point colors have the same value in their alpha channel (`.a`), the final accumulated alpha acts as a (possibly scaled) count of how many points have ever reached a given position, while the other channels represent the actual color data.

## Histogram Usage
As frames occur, the histogram is gradually converted (non-destructively) into an image through a process called **Log-Density Display**. The outline of this process is described in the [original fractal flame paper by Scott Draves](https://flam3.com/flame_draves.pdf). Briefly, it consists of two main steps:

1. A normalized histogram *N* is created by dividing every color in the histogram by the global maximum alpha channel value anywhere in the histogram.
2. An image *C* is created by taking a (scaled, adjusted) logarithm of the normalized values in *N*. The logarithm preserves finer details that a linear scale would miss.

The resulting image *C* is then displayed directly to the screen. For instructions on modifying this process, see [here](./modifying-the-log-density-display.md).