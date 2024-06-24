# ``SwiftDocCRender``

A web renderer for DocC Archives produced by DocC.

## Overview

DocC-Render displays documentation produced by [DocC](https://www.swift.org/documentation/docc) on the web and is included in DocC Archives.

### How It Works

DocC-Render is a Single Page Application (SPA for short), powered by [Vue.js](https://vuejs.org). SPAs are web apps that are rendered entirely in the browser using JavaScript. Pages and content are generated dynamically at runtime, based on DocC generated Render JSON data.

### New Components

#### Latex

This implementation of LaTeX hijacks the `math` code block language to render LaTeX code. This implementation 
is limited to display math mode and does not support inline math mode. Multiline LaTeX code blocks are also supported.

```math
c = \pm\sqrt{a^2 + b^2}

d = \pm\sqrt{e^2 + f^2}

g = \pm\sqrt{h^2 + i^2}
```

#### Colourful Asides

> Note: Notes are gray

> Important: Importants are yellow

> Warning: Warnings are red

> Tip: Tips are green

> Experiment: Experiments are blue

## Topics

### Contributing and internals

- <doc:Internals>

### Commonly used components

- ``ContentNode``
- ``ContentNode/Reference``
- ``WordBreak``

<!-- Copyright (c) 2021 Apple Inc and the Swift Project authors. All Rights Reserved. -->
