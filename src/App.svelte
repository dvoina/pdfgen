<script>
  // import fabric from "fabric";
  import {onMount} from 'svelte';
  import {fabric} from 'fabric';
  import {jsPDF} from "jspdf";

  let canvas;
  let svg = "";
  let disabled;

  onMount(() => {
    canvas = new fabric.Canvas("c");
  })

  function selectText() {
    console.log("text");
    let text = new fabric.Text("Lorem Ipsum");
    canvas.add(text);
  }

  function selectCircle() {
    console.log("circle");
    let text = new fabric.Circle({color: "red", radius:"50"});
    canvas.add(text);
  }

  function notImplemented() {
    alert("Not implemented yet.")
  }

  function createSVG() {
    svg = canvas.toSVG({viewbox: {x:0, y:0, width:300, height:400}, width:410, height:310});
    console.log(svg);
  }

  async function createPDF() {
    var pdf = new jsPDF({
          orientation: 'p',
          unit: 'mm',
          format: 'a5',
          putOnlyUsedFonts:true
          });
      pdf.text("below should be svg", 20, 20);
      await pdf.addSvgAsImage(svg, 20, 30, 160, 120, "content", true, 0);
      await pdf.save('demo.pdf');
  }

  $: {disabled = !(svg.length>0);
    console.log(disabled);
  };
</script>

<main>

  <div class="svg">
    <canvas id="c"></canvas>
    <div id="toolbox">
      <ul class="tools">
        <li on:click={selectText}>Text</li>
        <li on:click={notImplemented}>Table</li>
        <li on:click={selectCircle}>Circle</li>
      </ul>
    </div>
  </div>
  <div>
    <button on:click={createSVG}>Create SVG</button>
    <button on:click={createPDF} {disabled}>Create PDF</button>
  </div>
  <textarea class="svg-output" bind:value={svg}></textarea>
</main>

<style>
  .svg-output {
    width:120mm;
    height: 80mm;
  }
  .tools {
    list-style: none;
  }
  .tools > li {
    display:block;
    border: 1px solid purple;
    width: 10mm;
    height: 10mm;
    margin: 1mm;

  }
  .svg {
    display: flex;
  }

  #c {
    width:160mm;
    height:120mm;
    border: 1px solid purple;
  }
</style>
