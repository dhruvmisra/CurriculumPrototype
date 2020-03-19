<template>
  <div class="sidepanel border shadow">
    <div class="category-boxes">
      <div
        class="category-box border p-3"
        :class="{ selected: selectedCategory == i }"
        :style="{ background: category.bg }"
        v-for="(category, i) in categories"
        :key="category.title"
        @click="$emit('select', category.id)"
      >
        <i class="far fa-book-open"></i>
      </div>
    </div>

    <div class="profile">
      <img src="../assets/icons/owner-icon.svg" alt="profile-photo" class="profile-photo w-100">

      <p class="text-center my-0"> <i class="fas fa-folder text-primary" style="font-size: 20px"></i> {{ documents.length }}</p>
      <div class="documents">
        <div class="border" v-for="(doc, i) in documents" :key="i">
          <img :src="doc" alt="doc" v-if="doc[0] == 'd'" class="w-50">
          <audio :src="doc" v-if="doc[0] == 'b'" controls class="upload-audio w-50"></audio>
          <span style="font-size:10px">[Upload Time]</span>
        </div>
      </div>
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
    selectedCategory: Number,
    student: Object
  },
  data() {
    return {

    };
  },
  computed: {
    documents() {
      let documents = [];
      for(let cat in this.student.acquired) {
        for(let skill in this.student.acquired[cat]) {
          if(this.student.acquired[cat][skill].image != null) {
            documents.push(this.student.acquired[cat][skill].image);
          }
          if(this.student.acquired[cat][skill].audio != null) {
            documents.push(this.student.acquired[cat][skill].audio);
          }
        }
      }
      return documents;
    }
  },
  methods: {

  }
};
</script>

<style>
.sidepanel {
  width: 250px;
  height: 100%;
  display: flex;
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
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.category-box:hover {
  filter: brightness(90%);
}
.selected {
  filter: brightness(80%);
}

.profile {
  width: 100%;
}
.profile-photo {
  padding: 10px;
  border-radius: 20px;
}
.documents {
  padding:10px;
  height: 60vh;
  overflow-y: auto;
}
.documents img {
  padding: 10px;
}
</style>