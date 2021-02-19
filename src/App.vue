<template>
  <main>
    <h1>🍺getting the beers in</h1>
    <div id="top-section">
      <fave-beers :beers="faveBeers"></fave-beers>
    </div>
    <beer-list :beers="beers"></beer-list>
    <beer-detail :beer="selectedBeer" :isFave="faveBeers.indexOf(selectedBeer) > -1"></beer-detail>
  </main>
</template>

<script>
import { eventBus } from './main';
import BeerList from './components/BeerList';
import BeerDetail from './components/BeerDetail';
import FaveBeers from './components/FaveBeers';

export default {
  name: 'app',
  data() {
    return {
      beers: [],
      selectedBeer: null,
      faveBeers: [],
      sortByName: function(first, next) {
        if (first.name < next.name) {
          return -1;
        } else if (first.name > next.name) {
          return 1;
        } else {
          return 0;
        }
      }
    }
  },
  components: {
    "beer-list": BeerList,
    "beer-detail": BeerDetail,
    "fave-beers": FaveBeers
  },
  methods: {
    fetchAllBeers() {
      let promises = [1, 2, 3, 4, 5].map(fetchNum => {
        return fetch(
          `https://api.punkapi.com/v2/beers?page=${fetchNum}&per_page=80`
          ).then(res => res.json());
      });

      Promise.all(promises).then(data => {
        this.beers = data.flat();
      })
    }
  },
  mounted() {
    this.fetchAllBeers();
    // // fetch the first 80 beers by ID:
    // fetch('https://api.punkapi.com/v2/beers?per_page=80')
    // .then(apiResponse => apiResponse.json())
    // .then(beers => this.beers = beers.sort(this.sortByName));

    eventBus.$on('beer-selected', (beer) => {
      this.selectedBeer = beer;
    });

    eventBus.$on('close-pop', (should_close) => {
      if (should_close) {
        this.selectedBeer = null;
      }
    });

    eventBus.$on('beer-favourited', (beer) => {
      if (this.faveBeers.indexOf(beer) < 0) {
        this.faveBeers.push(beer);
      }
    });

    eventBus.$on('beer-unfavourited', (beer) => {
      let faveBeerIndex = this.faveBeers.indexOf(beer);
      if (faveBeerIndex > -1) {
        this.faveBeers.splice(faveBeerIndex, 1);
      }
    });
  }
}
</script>

<style>
  body {
    font-family: Arial, Helvetica, sans-serif;
  }
  h1 {
    text-align: center;
  }

  #top-section {
    width: 100%;
    display: flex;
    height: fit-content;
    justify-content: center;
  }
</style>