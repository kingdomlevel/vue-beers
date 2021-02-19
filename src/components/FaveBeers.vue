<template>
    <div v-if="beers.length !== 0" id="fave-beers" :style="{ transform: 'rotate('+ randomRotate+'deg)'}">
        <h2>Your Fave Beers</h2>
        <ul>
            <li v-for="(beer, index) in beers" :key="index">
                <span v-on:click="showDetails(beer)">{{beer.name}}</span>
                <span class="remove-button" v-on:click="unfaveBeer(beer)">❌</span>
                </li> 
        </ul>
    </div>
</template>

<script>
import {eventBus} from '../main';

export default {
    name: 'fave-beers',
    props: ['beers'],
    data() {
      return {
        randomRotate: Math.random() * 10 - 5
      }
  },
  methods: {
      unfaveBeer(currentBeer) {
          eventBus.$emit('beer-unfavourited', currentBeer);
      },
      showDetails(currentBeer) {
          eventBus.$emit('beer-selected', currentBeer);
      }
  }
}
</script>

<style scoped>
    #fave-beers {
        display: flex;
        flex-flow: column wrap;
        height: fit-content;
        width: 50%;
        right: 40%;
        top: 0;
        border: solid 4px black;
        margin-bottom: 20px;
    }

    #fave-beers h2 {
        text-align: center;
    }

    #fave-beers ul {
        margin: 0 0 20px 30px;
        list-style-type: none;
    }

    .remove-button {
        cursor: pointer;
        margin-left: 5px;
    }
</style>