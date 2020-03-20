<template>
  <div id="app">
    <Toolbar
      :settingsOpen="settingsOpen"
      :catSortable="catSortable"
      @settingsClicked="settingsClicked"
      @reorderClicked="reorderClicked"
      @downloadClicked="downloadClicked"
       />
    <Student
      :categories="categories"
      :student="student"
      :settingsOpen="settingsOpen"
      :catSortable="catSortable"
      @updateCategories="updateCategories"
      @addSkill="addSkill"
      @addCategory="addCategory" 
    />
    <Generate 
      ref="generate"
      :student="student"
      :categories="categories"
    />
  </div>
</template>

<script>
import Toolbar from './components/Toolbar/Toolbar'
import Student from "./components/Student";
import Generate from "./components/Generate";

export default {
  name: "App",
  components: {
    Student,
    Toolbar,
    Generate
  },
  data() {
    return {
      settingsOpen: false,
      catSortable: false,
      categories: [
        {
          id: 0,
          title: "Category 1",
          bg: '#4AC1F6',
          icon: 'book',
          skills: [
            {
              id: 0,
              title: "Skill 1",
              image: "d1.jpg"
            },
            {
              id: 1,
              title: "Skill 2",
              image: "d2.jpg"
            }
          ]
        },
        {
          id: 1,
          title: "Category 2",
          bg: '#77ffa0',
          skills: [
            {
              id: 0,
              title: "Skill 3",
              image: "d3.jpg"
            }
          ]
        },
        {
          id: 2,
          title: "Category 3",
          bg: '#a077ff',
          skills: [
            {
              id: 0,
              title: "Skill 4",
              image: "d4.jpg"
            },
            {
              id: 1,
              title: "Skill 5",
              image: "d5.jpg"
            }
          ]
        }
      ],
      student: {
        name: 'ABC',
        age: 10,
        rollNo: 0,
        acquired: {
          0: {
            0: {
              acquired: true,
              image: null,
              audio: null
            },
            1: {
              acquired: false,
              image: null,
              audio: null
            },
          },
          1: {
            0: {
              acquired: false,
              image: null,
              audio: null
            },
          },
          2: {
            0: {
              acquired: false,
              image: null,
              audio: null
            },
            1: {
              acquired: true,
              image: null,
              audio: null
            },
          }
        }
      },
    };
  },
  methods: {
    settingsClicked() {
      this.settingsOpen = !this.settingsOpen;
    },
    reorderClicked() {
      this.catSortable = !this.catSortable;
    },
    downloadClicked() {
      this.$refs.generate.generate();
    },
    updateCategories(categories) {
      this.categories = categories;
    },
    addSkill(skill, selectedCategory) {
      this.categories[selectedCategory].skills.push(skill);
      this.$set(this.student.acquired[selectedCategory], skill.id, {
        acquired: false,
        image: null,
        audio: null
      });
    },
    addCategory(cat) {
      this.categories.push(cat);
      this.$set(this.student.acquired, cat.id, {});
    }
  }
};
</script>

<style>
#app {
  
}
</style>
