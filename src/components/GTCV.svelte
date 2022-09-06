<script>
  import Grid from "./Grid.svelte";

  export let boxCount = "3";
  let valcol = 1;
  let maxcol = 4;
  let stepcol = 1;
  let valgap = 0;
  let maxgap = 20;
  let stepgap = 5;

  let node;
  let columnString = "1fr";

  function doStuff() {
    if (!node) {
      return;
    }
    columnString =
      [...node.querySelectorAll("input")].map((el) => el.value).join("fr ") +
      "fr";
  }
  function incr() {
    if (boxCount < 8) boxCount++;
  }
  function decr() {
    if (boxCount > 1) boxCount--;
  }
</script>

<section>
  <div class="code-section">
    <pre><code
        >.grid_{columnString.replaceAll("fr", "").replaceAll(" ", "-")} {"{"}
  display: grid;
  <span class="inline" bind:this={node}
          >grid-template-columns: <span class="output"
            >{#each { length: valcol } as _, i}
              <span class="group">
            <input type="number" min="1" max="5" value="1" on:input={doStuff} />
            <span>fr</span>
            </span>
            {/each}</span
          >;</span
        >
  gap: <span class="output">{valgap}px;</span>
{"}"}</code
      ></pre>
    <div class="control">
      <label for="b">Columns</label>
      <div class="inline">
        <span>1</span>
        <input
          id="b"
          type="range"
          bind:value={valcol}
          on:input={doStuff}
          step={stepcol}
          max={maxcol}
          min="1"
        />
        <span>{maxcol}</span>
      </div>
    </div>
    <div class="control">
      <label for="a">Gap</label>
      <div class="inline">
        <span>0px</span>
        <input
          id="a"
          type="range"
          bind:value={valgap}
          step={stepgap}
          max={maxgap}
        />
        <span>{maxgap}px</span>
      </div>
    </div>
    <div class="inline">
      <button on:click={decr}>Remove child</button>
      <button on:click={incr}>Add child</button>
    </div>
  </div>
  <div class="wrapper" style="--cols: {columnString}; --gap: {valgap}px;">
    <div
      class="parent"
      data-url={columnString.replaceAll("fr", "").replaceAll(" ", "-")}
    >
      {#each { length: boxCount } as _, i}
        <span class="child-{i + 1}">.child-{i + 1}</span>
      {/each}
    </div>
  </div>
</section>

<style>
  .code-section {
    display: grid;
    grid: auto-flow min-content / auto;
    gap: 2rem;
  }
  .inline {
    display: inline-flex;
    gap: 0.5ch;
    align-items: center;
  }

  .control {
    display: grid;
    gap: 3px;
    color: #fff;
    font-weight: 600;
  }

  input[type="range"] {
    display: block;
    accent-color: #fff;
    inline-size: 100%;
    max-inline-size: 300px;
  }

  .wrapper {
    padding: 2rem;
    background: #fff;
    max-inline-size: 100%;
    min-inline-size: 0;
  }

  .wrapper > div {
    display: block;
    /*gap: calc(var(--progress, 0) * var(--prop));*/
    place-items: stretch;
    background: repeating-linear-gradient(
        -45deg,
        #5000ca66 0 1.5px,
        #fff0 0 4.95px
      )
      0 0 content-box;
    position: relative;
    padding: 10px;
    border: 2px dashed #5000ca;
    border-radius: 2px;

    display: grid;
    grid-template-columns: var(--cols);
    gap: var(--gap, 0);
  }

  .wrapper > div::before {
    content: ".parent";
    content: ".grid_" attr(data-url);
    position: absolute;
    background: #fff;
    padding-inline: 0.25em;
    top: -0.8em;
    left: 0.3em;
    line-height: 1;
    font-size: 0.9em;
    color: #5000ca;
  }

  .wrapper span {
    display: grid;
    place-items: center;
    min-block-size: var(--h, 3.5rem);
    inline-size: 100%;
    background: #72f1b8;
    outline: 1px solid;
    outline-offset: -1px;
  }

  pre {
    background: #e1e3ea;
    padding: 2px 8px 4px;
    border-radius: 4px;
    line-height: 1.8;
  }

  code {
    font-family: monaco;
    font-weight: 600;
  }

  .output {
    background: #5000ca;
    color: #fff;
    display: inline-flex;
    gap: 1ch;
    align-items: center;
    border-radius: 4px;
    padding-inline: 0.25rem;
    margin-inline: -0.25rem;
  }

  .group {
    display: inline-flex;
    align-items: center;
    font-variant: tabular-nums;
  }
  input {
    width: 2ch;
    color: inherit;
    font: inherit;
    background: inherit;
    border: none;
  }

  pre input + span {
    margin-left: -0.4rem;
  }

  input[type="number"]::-webkit-inner-spin-button,
  input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
  }
  input[type="number"] {
    -moz-appearance: textfield;
  }
</style>
