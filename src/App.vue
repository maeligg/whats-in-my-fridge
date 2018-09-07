<template>
  <div id="app">
    <Fridge :ingredients="ingredients" @addIngredient="addIngredient" @removeIngredient="removeIngredient" />
    <button @click="fetchRecipes(ingredients)">Fetch recipes</button>
    <Recipes :fetchedRecipes="fetchedRecipes" :loadingRecipes="loadingRecipes" :recipes="recipes" />
  </div>
</template>

<script>
import Fridge from './components/Fridge.vue'
import Recipes from './components/Recipes.vue';
import axios from 'axios';

export default {
  name: 'app',
  components: {
    Fridge,
    Recipes
  },
  data: function() {
    return {
      loadingRecipes: false,
      fetchedRecipes: false,
      ingredients: [],
      recipes: []
    }
  },
  methods: {
    fetchRecipes: function (ingredients) {
      this.loadingRecipes = true;
      let url = 'https://spoonacular-recipe-food-nutrition-v1.p.mashape.com/recipes/findByIngredients?ingredients=';
      ingredients.forEach((ingredient, index) => {
        url += index > 0 ? ',' : '';
        url += ingredient;
      });
      url += '&number=5&ranking=1';

      const optionsRecipesList = {
        method: 'GET',
        headers: { 'X-Mashape-Key': 'sz1Nt4brIJmshZRjiuvX8KrAig8Bp1VkrByjsnKaLcnNgRwx4x'},
        url: url,
      };
      axios(optionsRecipesList)
      .then(response => {
        let data = response.data;
        console.log(data);
        
        data.forEach((recipe, index) => {
          const urlRecipe = `https://spoonacular-recipe-food-nutrition-v1.p.mashape.com/recipes/${recipe.id}/information`;
          const optionsRecipeInfo = {
            method: 'GET',
            headers: { 'X-Mashape-Key': 'sz1Nt4brIJmshZRjiuvX8KrAig8Bp1VkrByjsnKaLcnNgRwx4x'},
            url: urlRecipe
          }
          axios(optionsRecipeInfo)
          .then(response => {
            data[index].sourceUrl = response.data.sourceUrl;
          })
        });

        this.loadingRecipes = false;
        this.fetchedRecipes = true;
        console.log(data);
        this.recipes = data;
      })
      .catch((error) => {
        this.loadingRecipes = true;
        this.fetchedRecipes = true;
      });
    },
    addIngredient: function(newIngredient) {
      this.ingredients.push(newIngredient);
      this.fetchedRecipes = false;
    },
    removeIngredient: function(ingredientToRemove) {
      this.ingredients = this.ingredients.filter(ingredient => {
        return ingredient !== ingredientToRemove;
      });
      this.fetchedRecipes = false;
    },
  }
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
