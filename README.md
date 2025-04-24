# Swift-DocC-Render

This is my personal fork of Swift-DocC-Render, with additional features and improvements.

## Development

Use `npm run serve` to start a development server with hot reload. This should only be used for checking that
your code compiles. To preview the produced documentation, see the [Usage](#usage) section.

## Usage

1. Follow steps 1 and 2 of the installation instructions in the [Getting Started](#getting-started) section.
2. Use `npm run build` to build the project. This should produce a `dist` directory in the project.
3. Run `export DOCC_HTML_DIR="/path/to/swift-docc-render/dist"` in the terminal, where the path is the location 
of the `dist` directory in the project.

To preview the documentation, run `npm run docs` to open this project's documentation, using your compiled
renderer. This will open a preview server on http://localhost:8000/documentation/swiftdoccrender.

To use your renderer to build your own project, run `xcrun docc convert . --transform-for-static-hosting` 
with the flags you would normally use.

## Changes

### LaTeX Support

This repository adds support for LaTeX, using the [KaTeX](https://katex.org/) library.

To use LaTeX in your documentation, write a code block with the language set to `math`. These code
blocks must contain only one line of valid LaTeX code. You may use multiple lines, but note that empty
lines have weight and will be rendered as blank space.

### Colourful Asides

This repository adds support for colourful asides, which are styled notes that can be used to draw
attention to important information. The following asides are supported:
- Note: Grey (default for DocC)
- Important: Yellow
- Warning: Red
- Tip: Green
- Experiment: Blue

### External Link Call to Actions
Call to Actions, the buttons that appear at the top of the page, now open links in new tabs instead of
replacing the current page.

### Graphs

This repository adds support for rendering graphs using the [function-plot](https://mauriciopoppe.github.io/function-plot/) 
library. It supports all functionality of the library, because it exposes all parameters to the documentation
writer.

To use graphs in your documentation, write a code block with the language set to `graph`. The first line 
is the identifier of the graph, which is not shown to the user but is used to identify the graph. The
identifier must be unique betweeng graphs on the same page. The rest of the code block is as if it were
entered directly into the `functionPlot` function in JavaScript, excluding `target` because that is what 
the first line does. Example:

```
graph-example
yAxis: { domain: [-1, 1] },
xAxis: { domain: [8, 24] },
data: [
  {
    fn: 'sin(x)'
  }
]
```

This current implementation uses the _incredibly_ insecure `new Function()` constructor to evaluate the
code block. This is a security risk and is likely to be changed in the future.

### Code Backgrounds

Inline code `blocks` now have a background colour and border, to look more like a code block.

# Original README

Below is the original README from the Apple Swift-DocC-Render repository.

Swift-DocC-Render is a web Single Page Application (SPA) powered by [Vue.js](https://vuejs.org/) for creating rich code documentation websites. Pages and content are generated using render JSON data from DocC. It comes with a well suited design for documentation websites.
SPAs are web apps that render dynamically at runtime entirely in the browser, using JavaScript.

[Swift-DocC](https://github.com/apple/swift-docc) is a tool for building and previewing documentation, aimed at making it easy to generate reference documentation from frameworks and packages, as well as free-form articles and tutorials authored in Markdown. It produces a folder in the Documentation Archive format (extension `.doccarchive`), which contains a machine-readable output of the documentation as JSON data that Swift-DocC-Render uses to produce rendered documentation.

## Getting Started

> Note: requires [Node.js](https://nodejs.org/en/download/) v18
> and [npm](https://www.npmjs.com/package/npm) v9.5. An easy way to set these up is to install
> [nvm](https://github.com/nvm-sh/nvm) and run `nvm install` from within the Swift-DocC-Render
> repository. To use these versions as the default, add `--default` to the installation command.

1. **Download this repository and go to its folder**

    ```shell
    git clone https://github.com/apple/swift-docc-render.git
    cd swift-docc-render
    ```

2. **Install dependencies**

    ```shell
    npm install
    ```

3. **Run a local server with hot reload at [localhost:8080](http://localhost:8080/)**

    You may want to set a proxy to handle data requests while developing locally by setting the VUE_APP_DEV_SERVER_PROXY env variable to a documentation archive (.doccarchive or .docc-build) on your disk or served in a HTTP endpoint:

    ```shell
    VUE_APP_DEV_SERVER_PROXY=[path to documentation archive] npm run serve
    ```

    As an alternative you can just create a `.env.development.local` file on the root of the project to add the `VUE_APP_DEV_SERVER_PROXY` env variable so you don't have to set it in the `npm run serve` script each time.

## Using Swift-DocC-Render to render documentation

Follow [these steps](https://github.com/apple/swift-docc#using-docc-to-build-and-preview-documentation) to generate a documentation archive, set the path to your renderer and render locally your documentation using Swift-DocC-Render.

## Rendering and building docs

You need to have [DocC](https://swift.org/documentation/docc) installed, in order to preview and build documentation. Read [Getting Started with Swift](https://www.swift.org/getting-started/) to learn more.

To preview the docs, run `npm run docs:preview`. This will spawn a preview server on http://localhost:8000/documentation/swiftdoccrender.

To build the docs, run `npm run docs:build`.

## Bug Reports and Feature Requests

### Submitting a Bug Report

Swift-DocC-Render tracks all bug reports with [GitHub](https://github.com/apple/swift-docc-render/issues).
When you submit a bug report we ask that you follow the
Swift [Bug Reporting](https://swift.org/contributing/#reporting-bugs) guidelines
and provide as many details as possible.

If you can confirm that the bug occurs when using the latest commit of Swift-DocC
from the `main` branch (see [Building Swift-DocC-Render](/CONTRIBUTING.md#build-and-run-swift-docc-render)),
that will help us track down the bug faster.

### Submitting a Feature Request

For feature requests, please feel free to create an issue
on [GitHub](https://github.com/apple/swift-docc-render/issues/new?assignees=&labels=New+Feature&projects=&template=FEATURE_REQUEST.yml) with the `New Feature` type
or start a discussion on the [Swift Forums](https://forums.swift.org/c/development/swift-docc).

Don't hesitate to submit a feature request if you see a way
Swift-DocC-Render can be improved to better meet your needs.

All user-facing features must be discussed
in the [Swift Forums](https://forums.swift.org/c/development/swift-docc)
before being enabled by default.

## Contributing to DocC

Please see the [contributing guide](/CONTRIBUTING.md) for more information.

<!-- Copyright (c) 2021 Apple Inc and the Swift Project authors. All Rights Reserved. -->
