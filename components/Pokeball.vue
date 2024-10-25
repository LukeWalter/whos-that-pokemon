<script>
export default {
  props: {
    frame: {
      type: Number,
      default: 0,
    },
  },
  setup(props) {
    const pokeballCanvas = useTemplateRef("pokeballCanvas");
    const pokeballFrames = useTemplateRef("pokeballFrames");
    const framesCanvas = useTemplateRef("framesCanvas");
    
    const drawFrame = (n) => {
        const context = framesCanvas.value.getContext("2d");
        const pokeballData = context.getImageData(144 * n, 0, 144, 144);
        const draw = pokeballCanvas.value.getContext("2d");
        draw.putImageData(pokeballData, 0, 0);
    };

    const frame = computed(() => { return props.frame; });
    watch(frame, (newFrame) => {
      drawFrame(newFrame);
    });

    return { pokeballCanvas, pokeballFrames, framesCanvas, frame, drawFrame };
  },
  mounted() {
    this.framesCanvas.width = this.pokeballFrames.width;
    this.framesCanvas.height = this.pokeballFrames.height;
    const context = this.framesCanvas.getContext("2d", { willReadFrequently: true });
    context.drawImage(this.pokeballFrames, 0, 0, 432, 144);
    this.drawFrame(this.frame);
  }
};
</script>

<template>
  <div>
    <img hidden ref="pokeballFrames" src="~/assets/img/pokeballFrames.png"/>
    <canvas hidden class="fCanv" ref="framesCanvas" />
    <canvas ref="pokeballCanvas"/>
  </div>
</template>

<style scoped>
.fCanv {
  width: 432px;
  height: 144px;
}
canvas {
  width: 60px;
  height: 30px;
}
</style>