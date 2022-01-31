<template>
  <div class="canvas">
    <h1>{{ msg }}</h1>
    <p><a href="https://codesource.io/build-a-drawing-app-with-and-vuejs-html5-canvas/" target="_blank" rel="noopener">Lien vers le tutoriel</a></p>
    <canvas
      class="mycanvas"
      id="canvas"
      :width="width"
      :height="height"
      @mousedown.left="startPainting"
      @mouseup.left="finishedPainting"
      @mousemove="draw"
      @mouseover="finishedPainting"
    >
    </canvas>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';

@Options({
  props: {
    msg: String
  }
})
export default class HelloWorld extends Vue {
  msg!: string

  canvas!: HTMLCanvasElement
  ctx!: CanvasRenderingContext2D
  width: number = window.innerWidth-40
  height: number = window.innerHeight/2

  painting = false

  private setWidth(): void {
    this.width = document.documentElement.clientWidth-40;
  }

  private setHeight() {
    this.height = document.documentElement.clientHeight/2;
  }

  private setDimensions() {
    this.setWidth()
    this.setHeight()
  }

  private startPainting(e: PointerEvent) {
    this.painting = true
    this.draw(e)
  }
  private finishedPainting() {
    this.painting = false
    this.ctx.beginPath()
  }
  private draw(e: PointerEvent) {
    if (this.painting) {
      console.log("draw")
      this.ctx.lineWidth = 10;
      this.ctx.lineCap ="round"
      this.ctx.lineTo(e.clientX-20,e.clientY-200)
      this.ctx.stroke()

      this.ctx.beginPath()
      this.ctx.moveTo(e.clientX-20,e.clientY-200)
    }
  }
  
  mounted() {
    this.canvas = document.getElementById("canvas") as HTMLCanvasElement;
    this.ctx = this.canvas.getContext("2d") as CanvasRenderingContext2D;

    window.addEventListener('resize', this.setDimensions);
  } 
  
  unmounted() {
    window.removeEventListener('resize', this.setDimensions);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .mycanvas {
    border:2px solid #00FFF7;
    border-radius: 0.6rem;
    display: block;
    margin: 0 auto;
  }
</style>
