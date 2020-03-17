<template>
  <div id="student">
    <Skills :category="categories[selectedCategory]" @skillAdded="addSkill" @goBack="$emit('goBack')"  />
    <Sidepanel :categories="categories" :selectedCategory="selectedCategory" @update="updateCategories" @select="selectCategory" />
  </div>
</template>

<script>
import Sidepanel from './Student/Sidepanel';
import Skills from './Student/Skills';

export default {
  name: 'Student',
  components: {
    Sidepanel,
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
#student {
  display: flex;
}
</style>
