<template>
  <section id="beer-list">
    <beer-overview v-for="(beer, index) in beers" :beer="beer" :key="index">{{beer.name}}</beer-overview>
  </section>
</template>

<script>
import BeerOverview from "./BeerOverview";

export default {
  name: "beer-list",
  props: ["beers"],
  components: {
    "beer-overview": BeerOverview
  }
  
};
</script>

<style>
#beer-list {
  display: flex;
  flex-flow: row wrap;
}
</style>