<template>
  <div id="skills" :style="{ background: category.bg }">
    <!-- <button class="btn border m-2 mr-auto" @click="$emit('goBack')">
      <i class="fas fa-chevron-left text-white"></i>
    </button> -->
    <!-- <button class="btn border m-2 mr-auto" :class="{ on: settingsOpen }" @click="$emit('settingsClicked')">
      <i class="fas fa-cog" :class="{ 'text-white': !settingsOpen }"></i>
    </button> -->
    <h3 class="text-center text-white m-3">{{ category.title }}</h3>
    <div class="search mx-5 my-3">
      <input id="search" type="text" class="form-control" placeholder="Search" v-model="query" />
    </div>
    <draggable v-model="category.skills" :disabled="!settingsOpen">
      <transition-group class="skill-grid" name="grid" mode="out-in">
        <div
          class="skill m-4"
          v-for="(skill, i) in filteredSkills"
          :key="skill.title"
          data-toggle="modal"
          :data-target="settingsOpen ? '' : '#addAchievement'"
          @click="selectSkill(skill.id)"
        >
          <p class="text-center">{{ skill.title }}</p>
          <img :src="getImage(skill.image, i)" v-if="skill.image != ''" alt="skill-image" class="w-100">
          <i class="fas fa-thumbs-up" v-if="student.acquired[category.id] && student.acquired[category.id][skill.id].acquired"></i>
        </div>
      </transition-group>
    </draggable>
    <div class="skill add-skill m-4 mx-auto" data-toggle="modal" data-target="#addSkill" v-if="settingsOpen">
      <i class="fas fa-plus display-4"></i>
    </div>

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
            <div class="checkbox">
              <!-- <label>Acquire Skill? <input type="checkbox" @change="skillChange($event, category.skills[selectedSkill].id)"></label> -->
              <label>Acquire Skill? <input type="checkbox" v-model="skillAcquired"></label>
            </div>
            <div class="uploads" v-if="student.acquired[category.id] && student.acquired[category.id][selectedSkill] && (student.acquired[category.id][selectedSkill].image != null || student.acquired[category.id][selectedSkill].audio != null)">
              <img :src="student.acquired[category.id][selectedSkill].image" v-if="student.acquired[category.id][selectedSkill].image != null" class="upload-image w-100">
              <audio id="player" :src="student.acquired[category.id][selectedSkill].audio" v-if="student.acquired[category.id][selectedSkill].audio != null" controls class="upload-audio w-100"></audio>
            </div>
            <label for="image">Image</label>
            <input
              id="image"
              type="file"
              accept="image/*"
              class="form-control"
              @change="onImageChange"
            />
            <label for="audio">Audio</label>
            <input
              id="audio"
              type="file"
              accept="audio/*"
              capture
              class="form-control"
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

    <div class="modal fade" id="addSkill" tabindex="-1" role="dialog" aria-labelledby="addSkillLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addSkillLabel">Add Skill</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <input type="text" class="form-control" v-model="skillTitle" placeholder="Title">
            <label for="image">Image</label>
            <input
              id="image"
              type="file"
              accept="image/*"
              class="form-control"
              @change="onImageChange"
            />
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" data-dismiss="modal" @click="addSkill">Save changes</button>
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
    category: Object,
    settingsOpen: Boolean,
    student: Object
  },
  data() {
    return {
      query: "",
      skillTitle: '',
      imageFile: null,
      audioFile: null,
      selectedSkill: 0,
      skillAcquired: false
    };
  },
  watch: {
    category: function(newVal, oldVal) {
      if(typeof newVal.skills[0] != 'undefined') {
        this.selectedSkill = newVal.skills[0].id;
      }
    },
    settingsOpen: function(newVal, oldVal) {
      this.imageFile = null;
      this.audioFile = null;
      this.skillTitle = '';
    }
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
    getImage(image, i) {
      if(image.indexOf('data:image') != -1) {
        return image;
      } else {
        return require("@/assets/skills/" + image);
      }
    },
    addSkill() {
      if(this.skillTitle == '' && this.imageFile == null) return;

      let vm = this;
      let skillObj = {
        id: this.category.skills.length,
        title: this.skillTitle,
        image: ''
      }
      if(this.imageFile != null) {
        const reader = new FileReader();
        reader.readAsDataURL(this.imageFile);
        reader.onload = function () {
          skillObj.image = reader.result;
          vm.$emit('skillAdded', skillObj);
          vm.skillTitle = '';
          vm.imageFile = null;
        };
      } else {
        vm.$emit('skillAdded', skillObj);
        vm.skillTitle = '';
      }
    },
    selectSkill(i) {
      this.selectedSkill = i;
      if(this.student.acquired[this.category.id]) {
        if(typeof this.student.acquired[this.category.id][i] == 'undefined') {
        this.skillAcquired = false;
        } else {
          this.skillAcquired = this.student.acquired[this.category.id][i].acquired;
        }
      }
    },
    addAchievement() {
      let vm = this;

      if(this.imageFile == null && this.audioFile == null) {
        this.student.acquired[this.category.id][this.selectedSkill].acquired = this.skillAcquired;
        return;
      }

      if(this.imageFile != null) {
        const reader = new FileReader();
        reader.readAsDataURL(this.imageFile);
        reader.onload = function () {
          vm.student.acquired[vm.category.id][vm.selectedSkill].image = reader.result;
          vm.imageFile = null;
        };
      }

      if(this.audioFile != null) {
        const url = URL.createObjectURL(this.audioFile);
        // Do something with the audio file.
        vm.student.acquired[vm.category.id][vm.selectedSkill].audio = url;
        vm.audioFile = null;
      }

      this.student.acquired[this.category.id][this.selectedSkill].acquired = true;
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
  width: calc(100% - 200px);
  height: 100%;
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
  overflow-y: hidden;
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

.fa-thumbs-up {
  position: absolute;
  bottom: 10px;
  right: 10px;
  font-size: 2em;
  color: green;
  border: solid 2px green;
  border-radius: 50%;
  padding: 10px;
}
.on {
  background: white !important;
  color: black !important;
}

.add-skill {
  display: flex;
  justify-content: center;
  align-items: center;
  border-top: none;
}
.add-skill i {
  font-size: 8em;
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

.uploads {
  height: 30vh;
  overflow-y: auto;
}
.upload-image {
  padding: 10px;
}
</style>