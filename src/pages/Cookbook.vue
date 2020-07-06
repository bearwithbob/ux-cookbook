<template>
  <Layout>
    <div class="container">
      <div class="contact-header">
        <h1 class="contact-title">Recipes</h1>
      </div>
      <div class="list" id="recipes">
        <div class="filter noselect">
          <div class="styled-checkbox">
            <label>
              <input type="checkbox" v-model="selectedCuisines" value="User research" />
              <span>User Research</span>
            </label>
          </div>
          <div class="styled-checkbox">
            <label>
              <input type="checkbox" v-model="selectedCuisines" value="Content strategy" />
              <span>Content strategy</span>
            </label>
          </div>
          <div class="styled-checkbox">
            <label>
              <input type="checkbox" v-model="selectedCuisines" value="Visual design" />
              <span>Visual design</span>
            </label>
          </div>
          <div class="styled-checkbox">
            <label>
              <input type="checkbox" v-model="selectedCuisines" value="Accessibility" />
              <span>Accessibility</span>
            </label>
          </div>
          <div class="styled-checkbox">
            <label>
              <input type="checkbox" v-model="selectedCuisines" value="Data analysis" />
              <span>Data & analysis</span>
            </label>
          </div>
          <div class="styled-checkbox">
            <label>
              <input type="checkbox" v-model="selectedCuisines" value="UX fundamentals" />
              <span>UX fundamentals</span>
            </label>
          </div>
          <button v-on:click="clearFilters">Clear filters</button>
        </div>
        <!-- todo: insert a button that clears all checkboxes -->
        <div class="recipes">
          <ul class="recipes-list">
            <li
              class="recipe"
              v-for="recipe in filteredRecipes"
              :key="recipe.name"
            >{{ recipe.name }}</li>
          </ul>
        </div>
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
  },
  methods: {
    clearFilters() {
      var checkedCheckboxes = document.getElementsByTagName("input");
      for (var i = 0; i < checkedCheckboxes.length; ++i) {
        checkedCheckboxes[i].checked = false;
      }
      this.selectedCuisines = [];
    }
  }
};
</script>

<style scoped>
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.filter {
  overflow: hidden;
}

.styled-checkbox {
  margin: 6px;
  background-color: #efefef;
  border-radius: 4px;
  overflow: auto;
  float: left;
}

.styled-checkbox:hover {
  background: #333;
  color: #fff;
}

.styled-checkbox label span {
  text-align: center;
  padding: 8px 16px;
  display: block;
}

.styled-checkbox label span:hover {
  cursor: pointer;
}

.styled-checkbox label input {
  position: absolute;
  top: -20px;
}

.styled-checkbox input:checked + span {
  background-color: #333;
  color: #fff;
}

.recipes {
}

.recipes-list {
  padding: 0;
  list-style-type: none;
  display: grid;
  grid-column-gap: 16px;
  grid-row-gap: 16px;
  grid-template-columns: repeat(3, minmax(0, 1fr));
}

.recipe {
  display: inline-block;
  text-align: center;
  border-radius: 4px;
  padding: 96px 32px;
  background-color: #f9f9f9;
}
</style>

