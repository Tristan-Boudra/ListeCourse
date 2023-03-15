<template>
  <main>
    <header>
      <h1>Recipe Book</h1>
    </header>
    <form class="formAddRecipe" @submit.prevent="addRecipe">
      <input type="text" v-model="newRecipe" placeholder="Enter a new recipe...">
      <button class="btnBlue">Add</button>
    </form>
    <ul>
      <li class="oneRecipe" v-for="(recipe, index) in recipe" :key="index">
        <div v-if="editingRecipeIndex === index">
          <input type="text" v-model="editingRecipeValue" @keydown.enter="updateRecipe(index)" @keydown.esc="cancelEditing">
          <button class="btnWhite" @click="updateRecipe(index)">Save</button>
          <button class="btnBlue" @click="cancelEditing">Cancel</button>
        </div>
        <div v-else>
          {{ recipe }}
          <button class="btnWhite" @click="editRecipe(index)">Edit</button>
          <button class="btnBlue" @click="deleteRecipe(index)">Delete</button>
        </div>
      </li>
    </ul>
  </main>
</template>

<script>
  export default ({
    name: 'AddRecipe',
    data() {
        return {
          recipe: [],
          newRecipe: '',
          editingRecipeIndex: null,
          editingRecipeValue: '',
          recipeCount: 0
        }
    },
    methods: {
        addRecipe() {
            if (this.newRecipe.trim() !== '') {
            this.recipe.push(this.newRecipe.trim());
            this.newRecipe = '';
            this.updateRecipeCount();
            }
        },
        deleteRecipe(index) {
            this.recipe.splice(index, 1);
            this.updateRecipeCount();
        },
        editRecipe(index) {
            this.editingRecipeIndex = index;
            this.editingRecipeValue = this.recipe[index];
        },
        updateRecipe(index) {
            if (this.editingRecipeValue.trim() !== '') {
            this.recipe[index] = this.editingRecipeValue.trim();
            this.cancelEditing();
            this.updateRecipeCount();
            }
        },
        cancelEditing() {
            this.editingRecipeIndex = null;
            this.editingRecipeValue = '';
        },
        updateRecipeCount() {
            this.recipeCount = this.recipe.length;
        }
    }
  });
</script>

<style scoped>
.btnBlue {
  background-color: #1E3041;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}

.btnWhite{
  background-color: white;
  color: #1E3041;
  border: solid 1px #1E3041;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 10px;
}

.oneRecipe{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 4px 0;
  padding: 10px;
  border: solid 1px #1E3041;
  border-radius: 5px;
}

ul{
  list-style: none;
  margin-top: 30px;
}

.formAddRecipe{
  margin-top: 30px;
  display: flex;
}

.formAddRecipe input{
  margin-right: 10px;
  padding: 5px;
  border: solid 1px #1E3041;
  border-radius: 5px;
}
</style>
