<script lang="ts">
  import PatternComponent from "./Pattern.svelte";

  import { filterPattern } from "../helpers";

  import type { Pattern } from "../types";

  import { labelFilter, predictionFilter } from "../stores";

  export let patterns: Pattern[];
  export let width: number;
  export let model: string;

  $: patternsWithFilteredSamples = patterns
    .map((pattern) => {
      return {
        pattern: pattern,
        filteredSamples: filterPattern(
          pattern,
          $labelFilter,
          $predictionFilter
        ).samples.filter((sample) => sample.model === model),
      };
    })
    .filter((item) => item.filteredSamples.length > 0);
  $: patternWidth = Math.max(
    width / patternsWithFilteredSamples.length - 20,
    800
  );
</script>

{#each patternsWithFilteredSamples as item, patternIndex}
  <PatternComponent
    {patternIndex}
    pattern={item.pattern}
    expanded={true}
    {patternWidth}
    filteredSamples={item.filteredSamples}
  />
{/each}
