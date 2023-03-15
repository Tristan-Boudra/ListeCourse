<template>
  <main>
    <header>
      <h1>Recipe Book</h1>
    </header>
    <form class="formAddRecipe" @submit.prevent="addRecipe">
      <input type="text" v-model="newRecipe" placeholder="Enter a new recipe...">
      <button type="submit" class="btnBlue">Add</button>
    </form>
    <ul>
      <li class="oneRecipe" v-for="(recipe, index) in recipe" :key="index" @click="showRecipe(index)">
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
    <div v-if="selectedRecipe" class="modal">
      <div class="modal-content">
        <div v-if="editingRecipeIndex === index">
          <div class="updateRecipe">
            <div class="recipeInfoUpdate">
              <label for="title">Title</label>
              <input type="text" id="title" v-model="recipeInfo.title">
            </div>
            <div class="recipeInfoUpdate">
              <label for="description">Description</label>
              <textarea id="description" v-model="recipeInfo.description"></textarea>
            </div>
          </div>
          <button class="btnWhite" @click="updateRecipe(index)">Save</button>
          <button class="btnBlue" @click="cancelEditing">Cancel</button>
        </div>
        <div v-else class="recipeInfo">
          <div class="recipeHeader">
            <div class="recipeTitle">
              <!-- Affiche le titre de la recette -->
              {{ recipeInfo.title }}
            </div>
            <div class="recipeDescription">
              <!-- Affiche la description de la recette -->
              {{ recipeInfo.description }}
            </div>
          </div>
          <p class="recipeTitleIngredient">Ingredients:</p>
          <!-- Ajout nouvel ingrédient -->
          <form class="formAddRecipe" @submit.prevent="addIngredient">
            <input type="text" v-model="newIngredient" placeholder="Enter a new ingredient...">
            <button type="submit" class="btnBlue">Add</button>
          </form>
          <!-- Affiche les ingrédients de la recette -->
          <ul>
            <li class="oneRecipe" v-for="(ingredient, index) in ingredient" :key="index">
              <div>
                {{ ingredient }}
                <button class="btnBlue" @click="deleteIngredient(index)">Delete</button>
              </div>
            </li>
          </ul>
          <div class="buttonRecipe">
            <button class="btnWhite" @click="editRecipe(index)">Edit</button>
            <button class="btnBlue" @click="hideRecipe">Close</button> 
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
  export default ({
    name: 'AddRecipe',
    data() {
        return {
          recipe: [],
          ingredient: [],
          newRecipe: '',
          newIngredient: '',
          editingRecipeIndex: null,
          editingRecipeValue: '',
          recipeCount: 0,
          recipeInfo: [
            {
              title: '',
              description: '',
              ingredients: [
                {
                  name: '',
                  quantity: '',
                }
              ]
            }
          ],
          selectedRecipe: null,
        }
    },
    methods: {
      addRecipe() {
          if (this.newRecipe.trim() !== '') {
          this.recipe.push(this.newRecipe.trim());
          this.newRecipe = '';
          }
      },
      addIngredient() {
          if (this.newIngredient.trim() !== '') {
          this.ingredient.push(this.newIngredient.trim());
          this.newIngredient = '';
          }
      },
      deleteRecipe(index) {
          this.recipe.splice(index, 1);
      },
      deleteIngredient(index) {
          this.ingredient.splice(index, 1);
      },
      editRecipe(index) {
          this.editingRecipeIndex = index;
          this.editingRecipeValue = this.recipe[index];
      },
      updateRecipe(index) {
          if (this.editingRecipeValue.trim() !== '') {
          this.recipe[index] = this.editingRecipeValue.trim();
          this.cancelEditing();
          }
      },
      cancelEditing() {
          this.editingRecipeIndex = null;
          this.editingRecipeValue = '';
      },
      showRecipe(index) {
        // Verif si la recette existe
        if (this.recipe.length > index) {
          this.selectedRecipe = this.recipe[index];
          const modal = document.querySelector('.modal');
          modal.style.display = 'block';
        }
      },
      hideRecipe() {
          this.selectedRecipe = null;
          const modal = document.querySelector('.modal');
          modal.style.display = 'none';
      },
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
  background-color: white;
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

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  max-width: 500px;
}

.recipeInfo{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100%;
}

.recipeHeader{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100%;
}

.recipeTitle{
  font-size: 1.5em;
  font-weight: bold;
}

.recipeDescription{
  font-size: 1em;
  font-style: italic;
}

.recipeTitleIngredient{
  font-size: 1.2em;
  font-weight: bold;
  margin-top: 20px;
}

.buttonRecipe{
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.updateRecipe{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100%;
  margin-bottom: 10px;
}

.recipeInfoUpdate{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100%;
  margin-bottom: 10px;
}
</style>
