<template>
  <div id="teacher">
    <Sidebar :categories="categories" @update="updateCategories" @select="selectCategory" @goBack="$emit('goBack')" />
    <Skills :category="categories[selectedCategory]" @skillAdded="addSkill" />
  </div>
</template>

<script>
import Sidebar from './Teacher/Sidebar';
import Skills from './Teacher/Skills';

export default {
  name: 'Teacher',
  components: {
    Sidebar,
    Skills,
  },
  props: {
    categories: Array
  },
  data() {
    return {
      selectedCategory: 0
    }
  },
  methods: {
    updateCategories(cat) {
      this.$emit('updateCategories', cat);
    },
    selectCategory(i) {
      this.selectedCategory = i;
    },
    addSkill(skill) {
      this.categories[this.selectedCategory].skills.push(skill);
    }
  }
}
</script>

<style>
#teacher {
  display: flex;
}
</style>
