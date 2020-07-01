<template>
  <Layout>
    <div class="container">
      <div class="contact-header">
        <h1 class="contact-title">Recipes</h1>
      </div>
      <div class="list" id="people">
        <div class="filter">
          <label>
            <input type="checkbox" v-model="selectedCategories" value="Tech" /> Tech
          </label>
          <label>
            <input type="checkbox" v-model="selectedCategories" value="Entertainment" /> Entertainment
          </label>
          <label>
            <input type="checkbox" v-model="selectedCategories" value="Fictional" /> Fictional
          </label>
        </div>
        <!-- todo: insert a button that clears all checkboxes -->
        <ul class="people-list">
          <li v-for="person in filteredPeople" :key="person.name">{{ person.name }}</li>
        </ul>
      </div>
    </div>
  </Layout>
</template>

<script>
export default {
  data() {
    return {
      people: [
        { name: "Bill Gates", category: "Tech" },
        { name: "Steve Jobs", category: "Tech" },
        { name: "Jeff Bezos", category: "Tech" },
        { name: "George Clooney", category: "Entertainment" },
        { name: "Meryl Streep", category: "Entertainment" },
        { name: "Amy Poehler", category: "Entertainment" },
        { name: "Lady of Lórien", category: "Fictional" },
        { name: "BB8", category: "Fictional" },
        { name: "Michael Scott", category: "Fictional" }
      ],
      selectedCategories: []
    };
  },
  computed: {
    filteredPeople() {
      var categories = this.selectedCategories;
      if (!categories.length) {
        return this.people;
      } else {
        return this.people.filter(function(person) {
          return categories.includes(person.category);
        });
      }
    }
  }
};
</script>

<style scoped>
.contact-header {
  padding: 2rem 0 4rem 0;
}
.contact-title {
  font-size: 4rem;
  margin: 0 0 4rem 0;
  padding: 0;
}
.sender-info {
  display: flex;
  flex-wrap: wrap;
  margin-bottom: 2rem;
}
.sender-info > div {
  flex: 1;
  margin-right: 4rem;
}
.sender-info > div:last-of-type {
  margin: 0;
}
input:focus,
textarea:focus {
  border-color: var(--color-contrast-1);
}
input,
textarea {
  background: transparent;
  border: 1px solid var(--color-base-1);
  outline: none;
  border-radius: 0.3rem;
  padding: 0.8rem 1rem;
  color: inherit;
  font-size: 1rem;
  width: 100%;
}
textarea {
  resize: none;
  height: 140px;
}
.button {
  color: var(--color-base);
  background: var(--color-contrast);
  outline: none;
  border: 0;
  font-size: 0.8rem;
  padding: 0.8rem 1.6rem;
  border-radius: 0.3rem;
  margin-top: 2rem;
  cursor: pointer;
  transition: opacity 0.25s ease;
  font-size: 500;
  letter-spacing: 0.035em;
}
.button:hover {
  opacity: 0.6;
}
.button:focus {
  border: 1px solid var(--color-base-1);
}
</style>

