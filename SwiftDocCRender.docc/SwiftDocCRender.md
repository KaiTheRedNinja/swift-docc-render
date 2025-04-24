# ``SwiftDocCRender``

A web renderer for DocC Archives produced by DocC.

## Overview

DocC-Render displays documentation produced by [DocC](https://www.swift.org/documentation/docc) on the web and is included in DocC Archives.

### How It Works

DocC-Render is a Single Page Application (SPA for short), powered by [Vue.js](https://vuejs.org). SPAs are web apps that are rendered entirely in the browser using JavaScript. Pages and content are generated dynamically at runtime, based on DocC generated Render JSON data.

### New Components

#### Latex

This implementation of LaTeX hijacks the `math` code block language to render LaTeX code. 

To use multiline LaTeX, use the `math` code block language and write your LaTeX code inside. Each line of LaTeX code must be valid LaTeX code, 
and empty lines will be rendered as blank space.

```math
c = \pm\sqrt{a^2 + b^2}

d = \pm\sqrt{e^2 + f^2}

g = \pm\sqrt{h^2 + i^2}
```

To use inline LaTeX, use backticks (like you would for inline code), with dollar signs after the opening and before the closing backtick, and write your LaTeX code inside.
For example, `$a^2 + b^2 = c^2$`.

#### Colourful Asides

> Note: Notes are gray

> Important: Importants are yellow

> Warning: Warnings are red

> Tip: Tips are green

> Experiment: Experiments are blue

#### External Link Call to Actions
This can't really be demonstrated, trust me bro it works

#### Graphs

This implementation of graphs hijacks the `graph` code block language to render a graph, using the 
[function-plot](https://mauriciopoppe.github.io/function-plot/) library. This implementation exposes all
parameters to the documentation writer.

```graph
quadratic
yAxis: { domain: [-1, 9] },
grid: true,
data: [
    {
        fn: "x^2",
        derivative: {
            fn: "2 * x",
            updateOnMouseMove: true,
        },
    },
],
```

```graph
circle
xAxis: {
  label: 'real'
},
yAxis: {
  label: 'imaginary'
},
grid: true,
data: [{ fn: 'sqrt(1 - x * x)' }, { fn: '-sqrt(1 - x * x)' }]
```

#### Code Backgrounds

Inline code `blocks` now have a background colour and border, to look more like a code block.

## Topics

### Contributing and internals

- <doc:Internals>

### Commonly used components

- ``ContentNode``
- ``ContentNode/Reference``
- ``WordBreak``

<!-- Copyright (c) 2021 Apple Inc and the Swift Project authors. All Rights Reserved. -->
