<template>
  <div id="app">
    <component
      :is="currentComponent"
      :categories="categories"
      @goTo="goTo"
      @goBack="goBack"
      @updateCategories="updateCategories"
    ></component>
  </div>
</template>

<script>
import Home from "./components/Home";
import Teacher from "./components/Teacher";
import Student from "./components/Student";
import Classroom from "./components/Classroom";

export default {
  name: "App",
  components: {
    Home,
    Teacher,
    Student,
    Classroom
  },
  data() {
    return {
      currentComponent: "Home",
      categories: [
        {
          title: "Category 1",
          bg: '#4AC1F6',
          skills: [
            {
              title: "Skill 1"
            },
            {
              title: "Skill 2"
            }
          ]
        },
        {
          title: "Category 2",
          bg: '#77ffa0',
          skills: [
            {
              title: "Skill 1"
            }
          ]
        },
        {
          title: "Category 3",
          bg: '#a077ff',
          skills: [
            {
              title: "Skill 1"
            },
            {
              title: "Skill 2"
            }
          ]
        }
      ],
      selectedCategory: 0
    };
  },
  methods: {
    goTo(view) {
      this.currentComponent = view;
    },
    goBack() {
      this.currentComponent = "Home";
    },
    updateCategories(cat) {
      this.categories = cat;
    }
  }
};
</script>

<style>
#app {
  
}
</style>
