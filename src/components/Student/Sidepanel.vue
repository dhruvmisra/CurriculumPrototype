<template>
  <div class="sidebar border shadow">
    <div class="category-boxes">
      <div
        class="category-box border p-3"
        :class="{ selected: selectedCategory == i }"
        :style="{ background: category.bg }"
        v-for="(category, i) in categories"
        :key="category.title"
        @click="$emit('select', i)"
      ></div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: {
    draggable
  },
  props: {
    categories: Array,
    selectedCategory: Number
  },
  data() {
    return {

    };
  },
  // watch: {
  //   categories: function(newVal, oldVal) {
  //     this.cat = newVal;
  //   }
  // },
  methods: {

  }
};
</script>

<style>
.sidebar {
  width: 400px;
  height: 100vh;
}
.category-boxes {
  height: 100%;
  width: 60px;
  display: flex;
  flex-direction: column;
  cursor: pointer;
}
.category-box {
  height: 100%;
  width: 60px;
  cursor: pointer;
}
.category-box:hover {
  filter: brightness(90%);
}
.selected {
  filter: brightness(80%);
}
</style>