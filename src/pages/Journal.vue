<template>
  <Layout>
    <LatestJournals :journals="$page.posts.edges.slice(0,4)" />
    <div class="journal-title container">
      <span class="label">All recipes</span>
    </div>
    <div class="container">
      <div class="journal-hero">
        <!-- <h1 class="journal-header">a wise person once said...</h1> -->
      </div>

      <div class="filter noselect">
        <div v-for="item in allCuisines" :key="item.id" class="styled-checkbox">
          <label>
            <input type="checkbox" v-model="selectedCuisines" :value="item" />
            <span>{{ item }}</span>
          </label>
        </div>
      </div>

      <div class="recipes">
        <ul class="recipes-list">
          <g-link
            v-for="recipe in filteredRecipes"
            :to="recipe.node.path"
            :key="recipe.node.title"
            class="recipe"
          >
            <li class="journal-card">
              <h2 class="journal-recipe">{{ recipe.node.title }}</h2>
              <span class="journal-cuisine">
                {{
                recipe.node.cuisines.join(", ")
                }}
              </span>
              <p class="journal-excerpt">{{ recipe.node.excerpt }}</p>
            </li>
          </g-link>
        </ul>
      </div>
    </div>
  </Layout>
</template>

<page-query>
query Journal {
	posts: allJournalPost {
    edges {
      node {
        id
        path
        title
        excerpt
        cuisines
      }
    }
  }
}
</page-query>

<script>
import LatestJournals from "@/components/LatestJournals";

export default {
  components: {
    LatestJournals
  },
  data() {
    return {
      recipes: [],
      selectedCuisines: []
    };
  },
  computed: {
    // Find each unique cuisine from recipe YAML and store into the `cuisines` array
    allCuisines() {
      var allCuisines = [];
      this.$page.posts.edges.forEach(function(e) {
        // For recipes with more than one cuisines, compare each cuisine with existing ones
        if (e.node.cuisines.length > 1) {
          let newCuisines = e.node.cuisines;
          newCuisines.forEach(function(c) {
            if (allCuisines.indexOf(c) == -1) {
              allCuisines.push(c);
            }
          });
        } else {
          let newCuisine = Object.values(e.node.cuisines).toString();
          if (allCuisines.indexOf(newCuisine) == -1) {
            allCuisines.push(newCuisine);
          }
        }
      });
      return allCuisines;
    },

    // Filter recipes by selection
    filteredRecipes() {
      var userSelected = this.selectedCuisines;
      this.getRecipes();
      // If no selection, show all recipes
      if (!userSelected.length) {
        return this.recipes;
      } else {
        return this.recipes.filter(function(recipe) {
          let currentCuisines = recipe.node.cuisines;
          return currentCuisines.some(c => userSelected.includes(c));
        });
      }
    }
  },
  methods: {
    // Find all recipes from GraphQL query
    getRecipes: function() {
      this.recipes = this.$page.posts.edges;
    }
  }
};
</script>

<style scoped>
.container.journal {
  max-width: 720px;
}
.journal-hero {
  padding: 1rem 0;
  text-align: center;
  color: var(--color-base-1);
}
.journal-title {
  margin-top: 6rem;
  font-size: 2.4rem;
  font-weight: 400;
}
.journal-header {
  font-size: 3rem;
  font-weight: 700;
  padding: 0;
  margin: 0;
}
.journal-post {
  display: block;
  padding: 2rem 0;
  text-decoration: none;
  transition: background 0.5s ease;
}
.journal-post > * {
  transition: transform 0.5s ease;
}
.journal-post:hover {
  background-color: var(--color-base-1);
}
.journal-post:hover > * {
  transform: translateX(4rem);
}
.journal-post h1,
.journal-post h2 {
  margin: 0;
  padding: 0;
}

.journal-card {
  text-align: left;
}

.journal-recipe {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.journal-excerpt {
  font-size: 0.875rem;
  color: #555 !important;
  display: flex;
  color: var(--color-contrast-1);
  line-height: 18px !important;
  margin-top: 1.5rem;
}

.journal-cuisine {
  font-size: 0.875rem;
}

@media (min-width: 560px) {
  .journal-post {
    padding: 3rem 0;
  }
}

@media (min-width: 860px) {
  .journal-post {
    padding: 5rem 0;
  }
}

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
  background: #666;
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
  background-color: #000;
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
  padding: 32px 32px;
  background-color: #f9f9f9;
  text-decoration: none;
}
</style>
