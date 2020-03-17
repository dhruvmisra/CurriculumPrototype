<template>
  <div class="sidebar border shadow">
    <div class="header row mx-0 align-items-center">
      <button class="btn border m-2 mr-auto" @click="$emit('goBack')">
         <i class="fas fa-chevron-left"></i>
      </button>
      <h6 class="text-center my-3 text-secondary mx-auto">Categories</h6>
      <button class="btn border m-2 ml-auto" :class="{ 'sortable': sortable }" @click="sortable = !sortable">
         <i class="fas fa-sort-size-down-alt"></i>
      </button>
    </div>
    <p class="text-muted text-center" style="font-size: 12px">(Press the button on the right to re-order)</p>
    <draggable v-model="cat" @update="onUpdate" :disabled="!sortable">
      <transition-group name="list">
        <div class="item border p-3" v-for="(category, i) in cat" :key="category.title" @click="$emit('select', i)">
          {{ category.title }}
          <i class="fas fa-grip-lines text-muted ml-auto" v-if="sortable"></i>
        </div>
      </transition-group>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  components: {
    draggable,
  },
  props: {
    categories: Array
  },
  data() {
    return {
      cat: [],
      sortable: false,
    }
  },
  mounted() {
    this.cat = this.categories;
  },
  // watch: {
  //   categories: function(newVal, oldVal) {
  //     this.cat = newVal;
  //   }
  // },
  methods: {
    onUpdate(e) {
      this.$emit('update', this.cat);
    }
  }
}
</script>

<style>
.sidebar {
  width: 300px;
  height: 100vh;
}
.item {
  display: flex;
  cursor: pointer;
}
.list-enter-active, .list-leave-active {
  transition: all 200ms;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.list-move {
  transition: transform 200ms;
}
.sortable {
  background: rgb(221, 221, 221) !important;
}
</style>