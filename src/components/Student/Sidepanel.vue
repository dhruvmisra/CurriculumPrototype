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
      ></div>
    </div>

    <div class="profile">
      <img :src="getImage(student.image)" alt="profile-photo" class="profile-photo w-100">
      <p class="text-center">{{ student.name }}</p>

      <div class="documents">
        <p class="text-center">{{ documents.length }} docuements</p>
        <div v-for="(doc, i) in documents" :key="i">
          <img :src="doc" alt="doc" v-if="doc[0] == 'd'" class="w-100">
          <audio :src="doc" v-if="doc[0] == 'b'" controls class="upload-audio w-100"></audio>
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
    getImage(image, i) {
      // if(this.student.acquired[this.category.id][i].image != null) {
      //   return this.student.acquired[this.category.id][i].image;
      // } else if(image.indexOf('data:image') != -1) {
      //   return image;
      // } else {
      //   }
      return require("@/assets/students/" + image);
    },
  }
};
</script>

<style>
.sidepanel {
  width: 400px;
  height: 100vh;
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
  height: 45vh;
  overflow-y: auto;
}
.documents img {
  padding: 10px;
}
</style>