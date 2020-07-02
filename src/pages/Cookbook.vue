<template>
  <Layout>
    <div class="container">
      <div class="contact-header">
        <h1 class="contact-title">Recipes</h1>
      </div>
      <div class="list" id="recipes">
        <div class="filter">
          <label>
            <input type="checkbox" v-model="selectedCuisines" value="User research" /> User Research
          </label>
          <label>
            <input type="checkbox" v-model="selectedCuisines" value="Content strategy" /> Content strategy
          </label>
          <label>
            <input type="checkbox" v-model="selectedCuisines" value="Visual design" /> Visual design
          </label>
          <label>
            <input type="checkbox" v-model="selectedCuisines" value="Accessibility" /> Accessibility
          </label>
          <label>
            <input type="checkbox" v-model="selectedCuisines" value="Data analysis" /> Data analysis
          </label>
          <label>
            <input type="checkbox" v-model="selectedCuisines" value="UX fundamentals" /> UX fundamentals
          </label>
        </div>
        <!-- todo: insert a button that clears all checkboxes -->
        <ul class="recipes-list">
          <li v-for="recipe in filteredRecipes" :key="recipe.name">{{ recipe.name }}</li>
        </ul>
      </div>
    </div>
  </Layout>
</template>

<script>
export default {
  data() {
    return {
      recipes: [
        { name: "Usability testing", categories: ["User research"] },
        { name: "Participant recruitment", categories: ["User research"] },
        { name: "User interview", categories: ["User research"] },
        { name: "Surveys", categories: ["User research"] },
        { name: "Journey mapping", categories: ["User research"] },
        { name: "Style guides", categories: ["Content strategy"] },
        { name: "Card sorting", categories: ["Content strategy"] },
        { name: "High-fidelity prototyping", categories: ["Visual design"] },
        { name: "Rapid prototyping", categories: ["Visual design"] },
        { name: "Competitive analysis", categories: ["Data analysis"] },
        { name: "Personas", categories: ["User research", "UX fundamentals"] }
      ],
      selectedCuisines: []
    };
  },
  computed: {
    filteredRecipes() {
      var userSelected = this.selectedCuisines;
      if (!userSelected.length) {
        return this.recipes;
      } else {
        return this.recipes.filter(function(recipe) {
          return recipe.categories.some(c => userSelected.includes(c));
        });
      }
    }
  }
};
</script>

<style scoped>
</style>

