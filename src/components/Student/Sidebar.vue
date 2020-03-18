<template>
  <div class="sidebar border shadow">
    <div class="header row mx-0 align-items-center">
      <div class="mr-auto">
        <button class="btn border m-1" @click="$emit('goBack')">
          <i class="fas fa-chevron-left"></i>
        </button>
        <button class="btn border m-1" data-toggle="modal" data-target="#addCategory">
          <i class="fas fa-plus"></i>
        </button>
      </div>
      <h6 class="text-center my-3 text-secondary mx-auto">Categories</h6>
      <button class="btn border m-2 ml-auto" :class="{ 'sortable': sortable }" @click="sortable = !sortable">
         <i class="fas fa-sort-size-down-alt"></i>
      </button>
    </div>
    <p class="text-muted text-center" style="font-size: 12px">(Press the button on the right to re-order)</p>
    <draggable v-model="cat" @update="onUpdate" :disabled="!sortable">
      <transition-group name="list">
        <div class="item border p-3" v-for="(category, i) in cat" :key="category.id" @click="$emit('select', category.id)">
          {{ category.title }}
          <i class="fas fa-grip-lines text-muted ml-auto" v-if="sortable"></i>
        </div>
      </transition-group>
    </draggable>

    <!-- Modal -->
    <div class="modal fade" id="addCategory" tabindex="-1" role="dialog" aria-labelledby="addCategoryLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addCategoryLabel">Add Category</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <input type="text" class="form-control my-2" v-model="categoryTitle" placeholder="Title">
            <input type="text" class="form-control my-2" v-model="categoryBg" placeholder="Background HEX">
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" data-dismiss="modal" @click="addCategory">Save changes</button>
          </div>
        </div>
      </div>
    </div>

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
      categoryTitle: '',
      categoryBg: ''
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
    },
    addCategory(e) {
      this.$emit('categoryAdded', { id: this.categories.length, title: this.categoryTitle, bg: this.categoryBg, skills: [] });
      this.categoryTitle = '';
      this.categoryBg = '';
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