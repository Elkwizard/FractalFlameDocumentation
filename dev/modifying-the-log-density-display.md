# Modifying the Log-Density Display

> [!NOTE] This process requires full familiarity with the premise of the histogram and log-density display process, as well as a local development setup of FractalFlame.
> For an explanation of these concepts, see [here for the histogram](./what-is-the-histogram.md) and [here for the local setup](./setting-up-fractalflame-locally.md).

Though large scale modifications to the display process are possible, this page will only explain how to make pixel-local changes to the image-creation algorithm. To replace the display process:

1. Navigate to `FractalFlame.ts` in your local copy of [FractalFlame](https://xentempest.github.io/FractalFlame/FractalFlame.html).
2. Define a function (here it will be named `displayShader`, but any name is permissible) to convert a histogram color to a display color. This should be in the global scope and should have the following signature:
	```ts
	/**
	 * Converts a histogram pixel to a screen pixel.
	 * @param histogramPixel - An accumulated Color value from the histogram
	 * @param maxAlpha - The highest alpha (.a) value in the histogram.
	 * @return A Color to display directly to the screen
	 */
	function displayShader(histogramPixel: Color, maxAlpha: number): Color;
	```
	To get exactly the current behavior (Log-Density Display), this function should be defined as follows:
	```ts
	function displayShader(histogramPixel: Color, maxAlpha: number): Color {
		 const colorScaleCoefficient = 1 / Math.max(
			histogramPixel.r,
			histogramPixel.g,
			histogramPixel.b
		);
		const density = histogramPixel.a / maxAlpha;
		// below is the "log" in Log-Density Display
		const alpha = (
			Math.log1p(density * LOG_NORMAL_FACTOR) / logScale
		) ** GAMMA;
		const logDensityScaleCoefficient = colorScaleCoefficient * alpha * 255;
		return new Color(
			pixel.r * logDensityScaleCoefficient,
			pixel.g * logDensityScaleCoefficient,
			pixel.b * logDensityScaleCoefficient,
			1 // note that the maximum alpha is 1, not 255
		);
	}
	```
3. Locate the final `for` loop in the function `animate()`, then replace it as follows:
	```ts
	// ...
	let partition = choosePartition();
	for (let i = partition[0]; i < partition[1]; i++) {
		const pixel = histogram[i];
		const imgDataIndex = i * PIXEL_CHUNK_SIZE;
		// == START REPLACING HERE ==
		// use the function you defined
		const shaded = displayShader(pixel, freqMax); 
		// copy the resulting Color into the screen pixel buffer
		imageData.data[imgDataIndex + 0] = shaded.r;
		imageData.data[imgDataIndex + 1] = shaded.g;
		imageData.data[imgDataIndex + 2] = shaded.b;
		imageData.data[imgDataIndex + 3] = shaded.a * 255;
		// == STOP REPLACING HERE ==
	}
	// ...
	```
4. Compile, then reload your local copy of FractalFlame in a browser to see the effect of the new display algorithm.