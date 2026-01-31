<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2021-2024 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<!--
  [KAI]: This uses the FunctionPlot library.
  It hijacks the CodeListing node to render a graph instead.
-->

<template>
  <div :id="graphId"></div>
</template>

<script>
import { onMounted } from 'vue';
import functionPlot from 'function-plot';

export default {
  name: 'FunctionPlot',
  props: {
    content: String,
    graphId: String,
  },
  setup(props) {
    onMounted(() => {
      try {
        // Create a new function with the raw parameters passed in the content prop
        // eslint-disable-next-line
        let plotConfig = { target: `#${props.graphId}`, xAxis: {}, yAxis: {}, data: [], annotations: [], tip: {} };

        // Validate that content exists and is not empty
        if (!props.content || props.content.length === 0) {
          return;
        }
        const lines = props.content.split('\n');

        // Process each line of the content prop
        lines.slice(1).forEach((line) => {
          // Ignore comment and empty lines
          if (line.trim().startsWith('//') || line.trim() === '') return;
          // If the line starts with #, it is trying to change the plotConfig directly
          if (line.trim().startsWith('#')) {
            const firstSpaceIndex = line.indexOf(' ');
            if (firstSpaceIndex === -1) {
              return; // Invalid line, skip
            }

            const rawPath = line.slice(1, firstSpaceIndex).trim();
            const rawValue = line.slice(firstSpaceIndex + 1).trim();

            const formattedValue = (() => {
              // Try to parse as JSON first
              try {
                return JSON.parse(rawValue);
              } catch {
                // If that fails, return as string/number/boolean
                if (rawValue === 'true') return true;
                if (rawValue === 'false') return false;
                if (!Number.isNaN(Number(rawValue))) return Number(rawValue);
                return rawValue;
              }
            })();

            // eslint-disable-next-line no-console
            console.log(`Setting plotConfig at path ${rawPath} to value:`, rawValue, formattedValue);

            const path = rawPath
              .replace(/^#/, '') // remove leading #
              .split('.')
              .map(p => (p.match(/^\d+$/) ? Number(p) : p));
            let current = plotConfig;

            for (let i = 0; i < path.length; i += 1) {
              const key = path[i];
              const nextKey = path[i + 1];

              // last key → assign
              if (i === path.length - 1) {
                current[key] = formattedValue;
                return;
              }

              // create missing structure
              if (current[key] == null) {
                current[key] = typeof nextKey === 'number' ? [] : {};
              }

              current = current[key];
            }

            current = formattedValue;
          } else {
            // Otherwise, treat it as a data function
            if (plotConfig.data == null) {
              plotConfig.data = [];
            }
            plotConfig.data.push({ fn: line.trim() });
          }
        });

        // eslint-disable-next-line no-console
        console.log('FunctionPlot configuration:', plotConfig);

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
