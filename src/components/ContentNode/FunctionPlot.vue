<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2021-2024 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<!-- [KAI]: This uses the FunctionPlot library. It hijacks the CodeListing node to render a graph instead. -->

<template>
  <div id="root"></div>
</template>

<script lang="ts">
import { onMounted } from 'vue';
import functionPlot from 'function-plot';

export default {
  name: 'FunctionPlot',
  props: {
    content: String,
  },
  setup(props) {
    onMounted(() => {
      try {
        // eslint-disable-next-line no-console
        console.log('Graph mount content: ', props.content);

        // Create a new function with the raw parameters passed in the content prop
        // eslint-disable-next-line no-new-func
        const plotConfigFunction = new Function(`
          return {
            target: "#root",
            ${props.content}
          };
        `);

        // Execute the function to get the config object
        const plotConfig = plotConfigFunction();

        // eslint-disable-next-line no-console
        console.log('Parsed json: ', plotConfig);

        // Call functionPlot with the generated configuration
        functionPlot(plotConfig);
      } catch (error) {
        // eslint-disable-next-line no-console
        console.error('Invalid JSON string passed to FunctionPlot component:', error);
      }
    });
  },
};
</script>

<style scoped>
/* You can add any styling here */
</style>
