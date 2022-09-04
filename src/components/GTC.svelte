<script>
  export let boxCount = "3";
  let valcol = 1;
  let maxcol = 4;
  let stepcol = 1;
  let valgap = 0;
  let maxgap = 20;
  let stepgap = 5;
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
        >.parent {"{"}
  display: grid;
  <span class="output">grid-template-columns:{" 1fr".repeat(valcol)}</span>
  <span class="output">gap: {valgap}px</span>
{"}"}</code
      ></pre>
    <div class="control">
      <label for="b">Columns</label>
      <input
        id="b"
        type="range"
        bind:value={valcol}
        step={stepcol}
        max={maxcol}
        min="1"
      />
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
  <div class="wrapper" style="--cols: {valcol}; --gap: {valgap}px">
    <div class="parent">
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
    display: flex;
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
    grid-template-columns: repeat(var(--cols), 1fr);
    gap: var(--gap, 0);
  }

  .wrapper > div::before {
    content: ".parent";
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
    padding-inline: 0.25rem;
    margin-inline: -0.25rem;
    border-radius: 4px;
    display: inline-block;
    font-variant: tabular-nums;
  }
</style>
