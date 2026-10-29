# System Architecture

> [!NOTE]
This page lays out the structure of [FractalFlame](https://xentempest.github.io/FractalFlame/FractalFlame.html)'s TypeScript implementation, but assumes familarity with the software's purpose and algorithm. For information on the mathematical underpinnings, see [here](../usr/what-is-a-fractal-flame.md), and for new users, see [here](../usr/what-is-fractalflame.md).

## Overview
FractalFlame is implemented as a minimalist [TypeScript](https://www.typescriptlang.org/) project to be run directly either locally as file or as a locally- or remotely-hosted website. Its functionality is split into conceptually distinct files, which are globally included `<script>` tags. For information on setting up a valid development environment, see [here](./set-up-fractalflame.md).

