<script>
  import {
    BinningStore,
  } from '../../stores/binningStore.svelte.js';
  import CrosshairIcon from '$lib/icons/crosshair.svelte';

  let {
    binning,
  } = $props();
  
  let binWidthLocal = $state(0);
  let isInputValueValid = $derived(binWidthLocal > 0.0);
  /**
   * 
   * @param e {MouseEvent}
   */
  function killSelf(e) {
    // @ts-ignore
    BinningStore.deleteBinning(binning.id);
  }

  /**
   * @param e {Event}
  */
  function handleBinWidthChange(e) {
    const {
      target: {
        value,
      }
    } = e;

    binWidthLocal = value;

    if (isInputValueValid === false) {
      return;
    }

    BinningStore.updateBinning(binning.id, 'binWidth', parseFloat(value));
  }

  $effect(() => {
    console.log({ binning });
  });

  $effect(() => {
    console.log({ isInputValueValid });
  });
</script>

<style>
  .binning-settings-item {
    --padding: 0.5rem;

    display: grid;
    grid-template-columns: 5fr 1fr;
    grid-template-rows: auto 1fr;
    grid-template-areas:
      'binning-id kill'
      'properties kill'
    ;
    gap: 0.5rem;
    background-color: var(--theme-black);
    padding: var(--padding);
    border: var(--padding) solid var(--theme-green);
  }

  .binning-settings-id {
    grid-area: binning-id;
    display: flex;
    justify-content: end;
    align-items: center;
    padding: 0 calc(var(--padding) * 2);
  }

  .binning-settings-properties {
    grid-area: properties;
    display: grid;
    grid-auto-flow: row;
    gap: 0.5rem;
    padding: 0.5rem;
  }

  .binning-property {
    display: inline-grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: 1fr;
    grid-template-areas:
      'property-name property-value'
    ;
    gap: 0.5rem;
    padding: 0.5rem;
  }

  .property-name {
    grid-area: property-name;
    justify-content: start;
  }

  .property-value {
    grid-area: property-value;
    justify-content: stretch;
    font-size: 1.5rem;
    text-align: center;
    font-feature-settings: "frac", "tnum", "zero", "ss01";
    background-color: var(--theme-black);
    color: var(--theme-white);
    font-size: 2rem;
  }

  .property-value:invalid {
    background-color: var(--theme-red);
  }

  .property-name,
  .property-value {
    display: flex;
    align-items: center;
  }

  .binning-settings-kill {
    grid-area: kill;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .kill-button {
    display: flex;
    flex: 1 0 100%;
    aspect-ratio: 1 / 1;
    justify-content: center;
    align-items: center;
    font-size: 4rem;
    font-variation-settings: "wght" 900, "opsz" 32;
    background-color: var(--theme-black);
    color: var(--theme-green);
    border-radius: 0.25rem;
    cursor: pointer;
  }
</style>

<div class="binning-settings-item">
  <div class="binning-settings-id">{binning.id}</div>
  <div class="binning-settings-properties">
    <div class="binning-property">
      <label for="binWidth:{binning.id}" class="property-name">width</label>
      <input
        id="binWidth:{binning.id}"
        type="text"
        inputmode="decimal"
        class="property-value"
        value={binning.binWidth}
        pattern="\d+\.\d+"
        on:change={handleBinWidthChange}
      />
    </div>
  </div>
  <div class="binning-settings-kill">
    <button class="kill-button" on:click|preventDefault|stopPropagation|trusted={killSelf}>
      <CrosshairIcon />
    </button>
  </div>
</div> 