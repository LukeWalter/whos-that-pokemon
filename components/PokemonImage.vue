<script>
import { animate, spring, timeline } from "motion";
import { ref, useTemplateRef } from "vue";

export default {
  props: {
    name: {
      type: String,
      default: "bulbasaur",
    },
  },
  setup(props) {
    const lowerCaseName = props.name.toLowerCase();
    const capitalizedName = ref(
      lowerCaseName.charAt(0).toUpperCase() + lowerCaseName.slice(1)
    );

    const link = ref(
      "https://img.pokemondb.net/sprites/scarlet-violet/normal/" +
        lowerCaseName +
        ".png"
    );

    const released = ref(true);
    const animating = ref(false);

    const pokemon = useTemplateRef("pokemon");
    const pokeball = useTemplateRef("pokeball");
    const pokeballFrame = ref(0);

    const toggleState = () => {
      if (released.value && !animating.value) {
        animating.value = true;
        animate(pokemon.value, { cursor: "auto" }, { duration: 0 });
        console.log("" + capitalizedName.value + ", return!");
        animate(pokeball.value.$el, { opacity: 1 }, { duration: 0.2 }).finished.then(() => {
          pokeballFrame.value = 2;
          timeline([
            [pokemon.value, { filter: "brightness(0) invert(1)" }, { duration: 0.75 }],
            [pokemon.value, { width: "0px", height: "0px", opacity: 0, transform: "translateX(100%) translateY(-100%)", left: "50%" }, { duration: 0.5 }],
            [pokemon.value, { width: "256px", height: "256px", opacity: 0, transform: "translateX(0%) translateY(-50%)", left: "0%"  }, { duration: 0.1 }]
          ]).finished.then(() => {
            pokeballFrame.value = 0;
            timeline([
              [pokeball.value.$el, { top: "90%" }, { duration: 0.5, delay: 0.4, easing: [.8, 0, 1, 1]}],
              [pokeball.value.$el, { top: "75%" }, { duration: 0.3, easing: [0, .4, .6, 1]}],
              [pokeball.value.$el, { top: "90%" }, { duration: 0.3, easing: [.3, 0, 1, .7]}]
            ]).finished.then(() => {
              animating.value = false;
              released.value = !released.value;
              animate(pokemon.value, { cursor: "pointer" }, { duration: 0 });
            });
          });
        });
      } else if (!animating.value) {
        animating.value = true;
        animate(pokemon.value, { cursor: "auto" }, { duration: 0 });
        console.log("Go! " + capitalizedName.value + "!");
        pokeballFrame.value = 2;
        timeline([
          [pokemon.value, { opacity: 1 }, { duration: 0.4, delay: 0.2 }],
          [pokemon.value, { filter: "brightness(1)" }, { duration: 0.25, delay: 0.5 }],
        ]).finished.then(() => {
          pokeballFrame.value = 0;
          timeline([
            [pokeball.value.$el, { opacity: 0 }, { duration: 0.3, delay: 0.2 }],
            [pokeball.value.$el, { top: "50%" }, { duration: 0.1 }],
          ]).finished.then(() => {
            animating.value = false;
            released.value = !released.value;
            animate(pokemon.value, { cursor: "pointer" }, { duration: 0 });
          });
        });
      }
    };

    return { pokemon, pokeball, pokeballFrame, link, capitalizedName, toggleState };
  }
};
</script>

<template>
  <div class="imgBox" @click="toggleState">
    <Pokeball ref="pokeball" class="ball" :frame="pokeballFrame"/>
    <img
      ref="pokemon"
      class="mon"
      :src="link"
      :alt="capitalizedName"
    />
  </div>
</template>

<style scoped>
.imgBox {
  user-select: none;
  width: 256px;
  height: 256px;
  align-items: center;
  position: relative;
}
.mon {
  cursor: pointer;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: transparent;
  filter: brightness(1);
}
.ball {
  position: absolute;
  top: 50%;
  left: 45%;
  transform: translateY(-50%);
  opacity: 0;
}
</style>