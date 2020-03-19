<template>
  <div id="home">
    <h1 class="text-center text-white py-5">Curriculum Prototype</h1>

    <div class="views-container">
      <div class="views">
        <h5>Select View</h5>
        <button class="btn border" @click="$emit('goTo', 'Student')">Student's view</button>
        <button class="btn border" @click="$emit('goTo', 'Classroom')">Classroom view</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style>
#home {
  position: relative;
  height: 100vh;
  width: 100vw;
  background: rgb(33, 33, 165);
}
.views-container {
  height: 60%;
  display: flex;
  justify-content: center;
  align-items: center;  
}
.views {
  color: white;
  text-align: center;
}
.views .btn {
  width: 300px;
  background: rgb(209, 209, 209);
  padding: 20px 40px;
  margin: 20px;
  font-size: 1.5em;
}
.views .btn:hover {
  background: white;
  box-shadow: 0 10px 20px rgba(255, 255, 255, 0.171);
}
</style>