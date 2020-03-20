<template>
  <div id="student">
    <Sidebar
      :categories="categories"
      :sortable="catSortable"
      @update="updateCategories"
      @select="selectCategory"
      @categoryAdded="addCategory"
      @goBack="$emit('goBack')"
      v-if="settingsOpen"
    />
    <Skills
      :category="categories.find(e => e.id == selectedCategory)"
      :settingsOpen="settingsOpen"
      :student="student"
      @skillAdded="addSkill"
      @goBack="$emit('goBack')"
    />
    <Sidepanel
      :student="student"
      :categories="categories"
      :selectedCategory="selectedCategory"
      @select="selectCategory"
      v-if="!settingsOpen"
    />
  </div>
</template>

<script>
import Sidebar from "./Sidebar";
import Sidepanel from "./Sidepanel";
import Skills from "./Skills";

export default {
  name: "Student",
  components: {
    Sidebar,
    Sidepanel,
    Skills
  },
  props: {
    categories: Array,
    student: Object,
    settingsOpen: Boolean,
    catSortable: Boolean
  },
  data() {
    return {
      selectedCategory: 0
    };
  },
  mounted() {
    this.selectedCategory = this.categories[0].id;
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
      this.$emit('addSkill', skill, this.selectedCategory);
    }
  }
};
</script>

<style>
#student {
  height: calc(100vh - 55px);
  display: flex;
}
</style>
