# Adding a Non-Linear Function

> [!NOTE] This process requires familiarity with the mathematical basis of NLFs and a local development setup of FractalFlame.
> For an explanation of these concepts, see [here for NLFs](../usr/what-is-a-fractal-flame.md#nlf-vs-mat) and [here for the local setup](./setting-up-fractalflame-locally.md).

To add a new NLF, you will need to have its mathematical representation nearby. This will be a vector function from ℝ²&nbsp;→&nbsp;ℝ², which we will here call *f*&nbsp;(*p*). Additionally, it may have some number of configurable parameters. To add *f*&nbsp;(*p*) to [FractalFlame](https://xentempest.github.io/FractalFlame/FractalFlame.html):

1. Navigate to `NonLinearFunctions.ts` in your local copy of FractalFlame.
2. Add a new element to the array initializer for `nonLinearFunctions`. The element should be created as follows, with the `ALL_CAPS` placeholders being replaced in later steps:
	```ts
	new NonLinearFunction("YOUR_NLF_NAME", YOUR_NLF_IMPL)
	```
3. Replace `YOUR_NLF_NAME` with a unique name for your new NLF.
4. Replace `YOUR_NLF_IMPL` an arrow function which returns a `Vector2D` and implements *f*&nbsp;(*p*). The signature should be based on the coordinate system for your function:
	* For a **cartesian function**, write a function of the form `({ x, y }) => ...`.
	* For a **polar function**, write a function of the form `({ r, theta }) => ...`.
	* For access to the **entire input point**, simply use `p => ...`.
5. If your function has configurable parameters:
	1. Replace your current `YOUR_NLF_IMPL` expression with `params => YOUR_NLF_IMPL`. The `params` variable contains a list of numeric values for each of your configurable parameter and can be used within the body of your implementation.
	2. Decide on a range of possible values for each parameter, which must be centered at 0. Place the radii (equivalently: maxima) of these ranges into an array and add it as an additional argument to the constructor:
		```ts
		new NonLinearFunction("YOUR_NLF_NAME", params => /* ... */, [/* put radii here */])
		```
6. Compile, then reload your local copy of FractalFlame in a browser to see your new NLF now available at the bottom of the list.