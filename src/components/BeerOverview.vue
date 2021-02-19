<template>
  <div class="beer-overview" 
        v-on:click="beerClicked"
        :style="{ transform: `rotate(${randomRotate}deg)`}">
    <h3>{{beer.name}}</h3>
    <img v-if="beer.image_url" :src="beer.image_url" />
    <p v-if="!beer.image_url" class="beer-icon">🍺</p>
    <h4>{{beer.tagline}}</h4>
  </div>
</template>

<script>
import { eventBus } from '../main';

export default {
  name: "beer-overview",
  props: ["beer"],
  data() {
      return {
        randomRotate: Math.random() * 50 - 25
      }
  },
  methods: {
      beerClicked() {
          eventBus.$emit('beer-selected', this.beer);
      }
  }
};
</script>

<style>
.beer-overview {
  display: relative;
  width: fit-content;
  min-width: 260px;
  max-width: 300px;
  min-height: 200px;
  border: solid 2px black;
  margin: 5px;
  background-color: white;
  z-index: 0;
  transition: 0.2s ease;
}

.beer-overview:hover {
    z-index: 100;
    background-color: #eee;
    cursor: pointer;
}

.beer-overview h3,
h4 {
  margin: 5px auto;
  text-align: center;
}
.beer-overview img {
  max-height: 240px;
  display: block;
  margin: auto;
}

.beer-icon {
  font-size: 12em;
  padding: 0;
  margin: 0;
}
</style>