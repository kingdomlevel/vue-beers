<template>
    <section v-if="beer" id="beer-detail">
        <h3 id="beer-title">{{beer.name}}</h3>
        <div class="flex">
            <img v-if="beer.image_url" :src="beer.image_url" />
            <p v-if="!beer.image_url" class="beer-icon lg">🍺</p>
            <div id="beer-info">
                <p>{{beer.description}}</p>
                <p>ABV: {{beer.abv}}%</p>
                <p>First brewed: {{beer.first_brewed}}</p>
                <p id="ingredients">
                    Ingredients:
                    <ul>
                        <li v-for="(ingredientCategory, index) in Object.keys(ingredients)" :key="index">
                            {{ingredientCategory.charAt(0).toUpperCase() + ingredientCategory.substring(1)}}(s):
                            <ul>
                                <li v-for="(ingredient, i) in ingredients[ingredientCategory]" :key="i">
                                    {{ingredient}}
                                </li>
                            </ul>
                        </li>
                    </ul>
                </p> 
                <p v-if="showFaveButton" class="fave-button" v-on:click="faveClicked">👍 fave</p>
                <p v-if="!showFaveButton" class="fave-button" v-on:click="unFaveClicked">👎 un-fave</p>
            </div>
        </div>
    <p class="close-link" v-on:click="closePop">x</p>
    </section>
</template>

<script>
import {eventBus} from '../main';

export default {
    name: 'beer-detail',
    props: ['beer', 'isFave'],
    computed: {
        ingredients() {
            let uniqueIngredients = {};
            for (let ingredientCategory in this.beer.ingredients) {
                if (typeof this.beer.ingredients[ingredientCategory] == "string") {
                    uniqueIngredients[ingredientCategory] = [
                        this.beer.ingredients[ingredientCategory]
                    ];   
                } else {
                    let ingredientCategoryData = this.beer.ingredients[ingredientCategory]
                    .map(ingredient => ingredient.name)
                    .filter(
                        (ingredientName, index, array) => 
                        array.indexOf(ingredientName) === index
                        );
                    uniqueIngredients[ingredientCategory] = ingredientCategoryData;
                }
            }
            return uniqueIngredients;
        },
        showFaveButton: {
            get: function() {
                return !this.isFave;
            },
            set: function(newVal) {
                this.isFave = newVal;
            }
        }
    },
    methods: {
        closePop() {
            eventBus.$emit('close-pop', true);
        },
        faveClicked() {
            this.showFaveButton = false;
            eventBus.$emit('beer-favourited', this.beer);
        },
        unFaveClicked() {
            this.showFaveButton = true;
            eventBus.$emit('beer-unfavourited', this.beer);
        }
    }
}
</script>

<style>
    #beer-detail {
        background-color: white;
        border: solid 4px black;
        z-index: 200;
        position: fixed;
        width: 600px;
        min-height: 650px;
        left: 50%;
        margin-left: -300px;
        top: 50%;
        margin-top: -350px;
    }

    .close-link {
        position: absolute;
        right: 0px;
        top: 0px;
        margin: 0px 20px;
        font-size: 1.5em;
    }

    .close-link:hover {
        cursor: pointer;
    }

    #beer-title {
        position: absolute;
        width: 100%;
        top: 0px;
        margin-top: 5px;
        text-align: center;
    }

    .flex {
        display: flex;
    }

    #beer-detail img {
        max-height: 340px;
        margin: 20% 60px;
    }

    #beer-info {
        margin: 30px;
    }

    #ingredients ul{
        margin: 2px;
    }

    .fave-button {
        border: solid 1px black;
        width: fit-content;
        padding: 0px 4px;
        font-weight: bold;
    }

    .fave-button:hover {
        cursor: pointer;
    } 

    .lg {
        font-size: 12em;
    }

</style>