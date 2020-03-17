<template>
  <div id="skills" :style="{ background: category.bg }">
    <button class="btn border m-2 mr-auto" @click="$emit('goBack')">
      <i class="fas fa-chevron-left text-white"></i>
    </button>
    <h3 class="text-center text-white m-3">{{ category.title }}</h3>
    <div class="search mx-5 my-3">
      <label for="search" class="text-white">Try searching a skill</label>
      <input id="search" type="text" class="form-control" placeholder="Search" v-model="query" />
    </div>
    <transition-group class="skill-grid" name="grid" mode="out-in">
      <div
        class="skill m-4"
        v-for="(skill, i) in filteredSkills"
        :key="skill.title"
        data-toggle="modal"
        data-target="#addAchievement"
        @click="selectedSkill = i"
      >
        <p class="text-center">{{ skill.title }}</p>
      </div>
    </transition-group>

    <!-- Modal -->
    <div
      class="modal fade"
      id="addAchievement"
      tabindex="-1"
      role="dialog"
      aria-labelledby="addAchievementLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addAchievementLabel">Add Achievement</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <label for="image">Image</label>
            <input
              id="image"
              type="file"
              accept="image/*"
              class="form-control"
              :disabled="audioFile != null"
              @change="onImageChange"
            />
            <label for="audio">Audio</label>
            <input
              id="audio"
              type="file"
              accept="audio/*"
              capture
              class="form-control"
              :disabled="imageFile != null"
              @change="onAudioChange"
            />
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button
              type="button"
              class="btn btn-primary"
              data-dismiss="modal"
              @click="addAchievement"
            >Save changes</button>
          </div>
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
    category: Object
  },
  data() {
    return {
      query: "",
      imageFile: null,
      audioFile: null,
      selectedSkill: 0
    };
  },
  computed: {
    filteredSkills() {
      let vm = this;
      return this.category.skills.filter(skill => {
        return skill.title.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1;
      });
    }
  },
  methods: {
    addAchievement() {
      this.$emit("skillAdded", { title: this.skillTitle });
      this.skillTitle = "";
    },
    onImageChange(event) {
      this.imageFile = event.target.files[0];
    },
    onAudioChange(event) {
      this.audioFile = event.target.files[0];
    },
  }
};
</script>

<style>
#skills {
  background: #4ac1f6;
  width: calc(100% - 400px);
  height: 100vh;
  overflow-y: auto;
}
.skill-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.skill {
  position: relative;
  width: 200px;
  height: 250px;
  padding: 1em;
  border-radius: 5px;
  background: white;
  border-top: solid 20px rgb(62, 62, 204);
  cursor: pointer;
  transition: all 200ms ease;
}
.skill:hover {
  transform: scale(0.98);
}
.grid-enter-active,
.grid-leave-active {
  transition: all 200ms;
}
.grid-enter,
.grid-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.grid-move {
  transition: transform 200ms;
}
</style>