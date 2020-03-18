<template>
  <div id="app">
    <component
      :is="currentComponent"
      :categories="categories"
      :students="students"
      @goTo="goTo"
      @goBack="goBack"
      @updateCategories="updateCategories"
      @addCategory="addCategory"
    ></component>
  </div>
</template>

<script>
import Home from "./components/Home";
import Student from "./components/Student";
import Classroom from "./components/Classroom";

export default {
  name: "App",
  components: {
    Home,
    Student,
    Classroom
  },
  data() {
    return {
      currentComponent: "Home",
      categories: [
        {
          id: 0,
          title: "Category 1",
          bg: '#4AC1F6',
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
      students: [
        {
          name: 'ABC',
          age: 10,
          rollNo: 0,
          image: 's1.jpg',
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
        {
          name: 'DEF',
          age: 10,
          rollNo: 1,
          image: 's2.jpg',
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
        {
          name: 'GHI',
          age: 10,
          rollNo: 2,
          image: 's3.jpg',
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
      ]
    };
  },
  methods: {
    goTo(view) {
      this.currentComponent = view;
    },
    goBack() {
      this.currentComponent = "Home";
    },
    updateCategories(categories) {
      this.categories = categories;
    },
    addCategory(cat) {
      this.categories.push(cat);
    }
  }
};
</script>

<style>
#app {
  
}
</style>
