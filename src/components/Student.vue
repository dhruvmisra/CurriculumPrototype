<template>
  <div id="student">
    <Sidebar
      :categories="categories"
      @update="updateCategories"
      @select="selectCategory"
      @categoryAdded="addCategory"
      @goBack="$emit('goBack')"
      v-if="settingsOpen"
    />
    <Skills
      :category="categories.find(e => e.id == selectedCategory)"
      :settingsOpen="settingsOpen"
      :student="students[0]"
      @skillAdded="addSkill"
      @settingsClicked="settingsOpen = !settingsOpen"
      @goBack="$emit('goBack')"
    />
    <Sidepanel
      :student="students[0]"
      :categories="categories"
      :selectedCategory="selectedCategory"
      @select="selectCategory"
      v-if="!settingsOpen"
    />
  </div>
</template>

<script>
import Sidebar from "./Student/Sidebar";
import Sidepanel from "./Student/Sidepanel";
import Skills from "./Student/Skills";

export default {
  name: "Student",
  components: {
    Sidebar,
    Sidepanel,
    Skills
  },
  props: {
    categories: Array,
    students: Array
  },
  data() {
    return {
      selectedCategory: 0,
      settingsOpen: false
    };
  },
  methods: {
    updateCategories(categories) {
      this.$emit("updateCategories", categories);
    },
    selectCategory(i) {
      this.selectedCategory = i;
    },
    addCategory(cat) {
      this.$emit("addCategory", cat);
    },
    addSkill(skill) {
      this.categories[this.selectedCategory].skills.push(skill);
    }
  }
};
</script>

<style>
#student {
  display: flex;
}
</style>
