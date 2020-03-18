<template>
  <div class="classroom">
    <button class="btn border m-2 mr-auto" @click="$emit('goBack')">
      <i class="fas fa-chevron-left text-white"></i>
    </button>
    <h1 class="text-center text-white">Classroom</h1>
    <div class="students">
      <div class="student" v-for="student in students" :key="student.rollNo">
        <img :src="getImage(student.image)" alt="">
        <p class="text-center">{{ student.name }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    students: Array
  },
  methods: {
    getImage(image) {
      return require("@/assets/students/" + image);
    },
  }
}
</script>

<style>
.classroom {
  background: rgb(91, 181, 241);
  color: white;  min-height: 100vh;
}

.students {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}
.student {
  background: white;
  padding: 10px;
  border-radius: 10px;
  color: black;
  cursor: pointer;
}
.student img {
  width: 200px;
}
</style>