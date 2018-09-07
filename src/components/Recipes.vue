<template>
  <div>
    <div class="lds-ring" v-if="loadingRecipes">
      <div></div>
      <div></div>
      <div></div>
      <div></div>
    </div>
    <div v-if="fetchedRecipes && recipes.length === 0">Sorry, I could not find any recipes matching your ingredients.</div>
    <ul class="recipes">
      <li v-for="(recipe, key) in recipes" class="recipe" :key="key">
        <h2>{{ recipe.title }}</h2>
        <img :src="recipe.image" alt="">
      </li>
    </ul>
  </div>
</template>

<script>
    export default {
        name: 'Recipes',
        props: {
          loadingRecipes: Boolean,
          fetchedRecipes: Boolean,
          recipes: Array
        }
    }
</script>

<style scoped>
.lds-ring {
  display: inline-block;
  position: relative;
  width: 64px;
  height: 64px;
}
.lds-ring div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 51px;
  height: 51px;
  margin: 6px;
  border: 6px solid #2c3e50;
  border-radius: 50%;
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: #2c3e50 transparent transparent transparent;
}
.lds-ring div:nth-child(1) {
  animation-delay: -0.45s;
}
.lds-ring div:nth-child(2) {
  animation-delay: -0.3s;
}
.lds-ring div:nth-child(3) {
  animation-delay: -0.15s;
}
@keyframes lds-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.recipes {
  display: flex;
  margin: 0;
}

.recipe {
  list-style: none;
}
</style>