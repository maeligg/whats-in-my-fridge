<template>
  <div class="hello">
    <div v-for="(ingredient, key) in ingredients" :key="key">
      <div class="ingredient">{{ingredient}}</div>
      <button @click="remove(ingredient)">-</button>
    </div>
    <div><input type="text" v-model="newIngredient" :placeholder="this.ingredientsAdded ? '' : 'tomatoes' ">
      <button @click="add">+</button>
    </div>
  </div>
</template>

<script>
import Ingredient from './Ingredient.vue';

export default {
  name: 'Fridge',
  components: {
    Ingredient
  },
  props: {
    ingredients: Array
  },
  data: function() {
    return {
      newIngredient: '',
      ingredientsAdded: false
    }
  },
  methods: {
    add: function() {
      if(this.newIngredient.length) {
        this.$emit('addIngredient', this.newIngredient);
        this.newIngredient = '';
      }

      if(!this.ingredientsAdded) {
        this.ingredientsAdded = true;
      }
    },
    remove: function(ingredientToRemove) {
      this.$emit('removeIngredient', ingredientToRemove);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ingredient {
  display: inline;
}
</style>
