# Setting Up FractalFlame Locally
To develop a version of [FractalFlame](https://xentempest.github.io/FractalFlame/FractalFlame.html), you will need the following software:

* The [Git](https://git-scm.com/) source control management system.
* A recent version of [Node.js](https://nodejs.org/en) and its package manager, `npm`.
* A global installation of the [TypeScript compiler](https://www.npmjs.com/package/typescript), `tsc`.

### To Get FractalFlame

1. Open a terminal in the location on your computer where you wish to place FractalFlame.
2. Clone the FractalFlame repository with the following command:
	```sh
	$ git clone "https://github.com/XenTempest/FractalFlame.git"
	```
3. Open the newly created `FractalFlame` directory in your text editor of choice.
4. Open the file `FractalFlame.html` in a browser.

> [!NOTE] Only Firefox and Chromium-based browsers (Chrome, Edge, Opera, etc) are officially supported.

### To Make Changes
1. Modify files as you please. See the [system architecture](./system-architecture.md) for the basics of what there is to change.
2. Compile the code by running the `tsc` command from within the `FractalFlame` directory. To recompile the code whenever it is changed, use `tsc --watch` instead.
3. Reload the `FractalFlame.html` file in the browser.