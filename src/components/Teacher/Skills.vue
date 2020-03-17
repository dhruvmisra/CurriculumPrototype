<template>
  <div id="skills" :style="{ background: category.bg }">
    <h3 class="text-center text-white m-3">{{ category.title }}</h3>
    <div class="search mx-5 my-3">
      <label for="search" class="text-white">Try searching a skill</label>
      <input id="search" type="text" class="form-control" placeholder="Search" v-model="query">
    </div>
    <draggable v-model="category.skills">
      <transition-group class="skill-grid" name="grid" mode="out-in">
        <div class="skill m-4" v-for="skill in filteredSkills" :key="skill.title">
          <p class="text-center">{{ skill.title }}</p>
        </div>
      </transition-group>
    </draggable>
    <div class="skill add-skill m-4 mx-auto" data-toggle="modal" data-target="#addSkill">
      <i class="fas fa-plus display-4"></i>
    </div>


    <!-- Modal -->
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
import draggable from 'vuedraggable'

export default {
  components: {
    draggable,
  },
  props: {
    category: Object
  },
  data() {
    return {
      query: '',
      skillTitle: '',
    }
  },
  computed: {
    filteredSkills() {
      let vm = this;
      return this.category.skills.filter(skill => {
        return skill.title.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1;
      });
    },
  },
  methods: {
    addSkill() {
      this.$emit('skillAdded', { title: this.skillTitle })
      this.skillTitle = '';
    }
  }
}
</script>

<style>
#skills {
  background: #4AC1F6;
  width: calc(100% - 200px);
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
.add-skill {
  display: flex;
  justify-content: center;
  align-items: center;
  border-top: none;
}
.add-skill i {
  font-size: 8em;
}
.add-skill:hover {
  transform: scale(0.98);
}
.grid-enter-active, .grid-leave-active {
  transition: all 200ms;
}
.grid-enter, .grid-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.grid-move {
  transition: transform 200ms;
}
</style>