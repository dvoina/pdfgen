<script>
  // import fabric from "fabric";
  import {onMount} from 'svelte';
  import {fabric} from 'fabric';
  import {jsPDF} from "jspdf";

  let canvas;
  let svg = "";
  let disabled;

  let foreignObject = 
  ` <foreignObject x="20" y="20" width="160" height="160">
<div xmlns="http://www.w3.org/1999/xhtml">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit.
  Sed mollis mollis mi ut ultricies. Nullam magna ipsum,
  porta vel dui convallis, rutrum imperdiet eros. Aliquam
  erat volutpat.
</div>`;


  onMount(() => {
    canvas = new fabric.Canvas("c", );
    canvas.setWidth(800);
    canvas.setHeight(600);
  })


  function getRndColor() {
    const choices=["red", "green", "blue", "orange", "pink"];
    var index = Math.floor(Math.random() * choices.length);
    return choices[index];
  }

  function selectText() {
    console.log("text");
    let text = new fabric.Text("Lorem Ipsum");
    text.fill = getRndColor();
    canvas.add(text);
    canvas.setActiveObject(text);
  }

  function selectCircle() {
    console.log("circle");
    let circle = new fabric.Circle({color: "red", radius:"50"});
    circle.fill = getRndColor();
    canvas.add(circle);
    canvas.setActiveObject(circle);
  }

  function selectTable() {
    alert("we will insert a placeholder for a svg <foreignobject>");
    // let text = new fabric.Object();
    // canvas.add(text);  
  }

  function createSVG() {
    svg = canvas.toSVG({viewbox: {x:0, y:0, width:300, height:400}, width:410, height:310, color:getRndColor()});
    //svg.replace("{{FOREIGN_OBJECT}}", foreignObject)
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
        <li><button on:click={selectText}>Text</button></li>
        <li><button on:click={selectTable}>Table</button></li>
        <li><button on:click={selectCircle}>Circle</button></li>
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
    width: 40mm;
    height: 10mm;
    margin: 1mm;
  }
  button {
    width:35mm;
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


