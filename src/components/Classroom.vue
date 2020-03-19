<template>
  <div class="classroom">
    <button class="btn border m-2 mr-auto" @click="$emit('goBack')">
      <i class="fas fa-chevron-left text-white"></i>
    </button>
    <h1 class="text-center text-white">Classroom</h1>
    <p class="text-center">Click on any student to generate a PDF report for their acquired skills. <br> (You are the first student [ABC] in Student View)</p>
    <p class="text-center"></p>
    <div class="students">
      <div
        class="student"
        v-for="(student, i) in students"
        :key="student.rollNo"
        @click="generatePDF(i)"
      >
        <img :src="getImage(student.image, 'students')" alt />
        <p class="text-center">{{ student.name }}</p>
      </div>
    </div>

    <div class="doc-container">
      <div id="doc" class="mt-2">
        <div class="profile text-center mx-auto">
          <h1 class="mt-5 pt-5">{{ students[selectedStudent].name }}</h1>
          <img
            :src="getImage(students[selectedStudent].image, 'students')"
            alt
            class="profile-photo d-block mx-auto"
          />
          <h5 class="my-3">Roll Number: {{ students[selectedStudent].rollNo }}</h5>
          <h5 class="my-3">Class: My Class</h5>
          <h5 class="my-3">Age: {{ students[selectedStudent].age }}</h5>
        </div>

        <hr class="my-5" />

        <div class="categories py-3">
          <div class="category my-5" v-for="category in categories" :key="category.id">
            <h1
              class="text-center my-4 py-2"
              :style="{borderBottom: 'solid 6px ' + category.bg}"
            >{{ category.title }}</h1>
            <div class="skills">
              <div class="skill-item my-3" v-for="skill in category.skills" :key="skill.id">
                <img :src="getImage(skill.image, 'skills')" alt class="skill-image" />
                <div class="uploaded w-100">
                  <div class="row mx-0 w-100" style="border-bottom: solid 2px grey">
                    <h6>{{ skill.title }}</h6>
                    <h5
                      class="ml-auto"
                      :class="{ 'text-success': students[selectedStudent].acquired[category.id][skill.id].acquired, 
                                'text-danger': !students[selectedStudent].acquired[category.id][skill.id].acquired }"
                    >{{ students[selectedStudent].acquired[category.id][skill.id].acquired ? 'Acquired' : 'Not Acquired' }}</h5>
                  </div>
                  <div class="uploaded" v-if="students[selectedStudent].acquired[category.id][skill.id].image != null || students[selectedStudent].acquired[category.id][skill.id].audio != null">
                    <img :src="students[selectedStudent].acquired[category.id][skill.id].image" v-if="students[selectedStudent].acquired[category.id][skill.id].image != null">
                    <div v-if="students[selectedStudent].acquired[category.id][skill.id].audio != null" class="m-5">[Audio added]</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import jsPDF from 'jspdf';
import html2canvas from 'html2canvas';

export default {
  props: {
    students: Array,
    categories: Array
  },
  data() {
    return {
      selectedStudent: 0
    };
  },
  methods: {
    getImage(image, folder) {
      if(image.indexOf('data:image') != -1) {
        return image;
      } else {
        return require("@/assets/" + folder + "/" + image);
      }
    },
    generatePDF(i) {
      this.selectedStudent = i;

      let vm = this;
      
      setTimeout(() => {
        let doc = document.getElementById('doc');
        var HTML_Width = doc.getBoundingClientRect().width;
        var HTML_Height = doc.getBoundingClientRect().height;
        var top_left_margin = 15;
        var PDF_Width = HTML_Width+(top_left_margin*2);
        var PDF_Height = (PDF_Width*1.5)+(top_left_margin*2);
        var canvas_image_width = HTML_Width;
        var canvas_image_height = HTML_Height;
        
        var totalPDFPages = Math.ceil(HTML_Height/PDF_Height)-1;
        html2canvas(doc,{allowTaint:true}).then(function(canvas) {
          canvas.getContext('2d');
          
          console.log(canvas.height+"  "+canvas.width);
          
          
          var imgData = canvas.toDataURL("image/jpeg", 1.0);
          var pdf = new jsPDF('p', 'pt',  [PDF_Width, PDF_Height]);
            pdf.addImage(imgData, 'JPG', top_left_margin, top_left_margin,canvas_image_width,canvas_image_height);
          
          
          for (var i = 1; i <= totalPDFPages; i++) { 
            pdf.addPage(PDF_Width, PDF_Height);
            pdf.addImage(imgData, 'JPG', top_left_margin, -(PDF_Height*i)+(top_left_margin*4),canvas_image_width,canvas_image_height);
          }
          
          pdf.save("Skills-" + vm.students[vm.selectedStudent].name + ".pdf");
        });
      }, 500);
    }
  }
};
</script>

<style>
.classroom {
  background: rgb(91, 181, 241);
  color: white;
  min-height: 100vh;
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
.doc-container {
  height: 0;
  overflow: hidden;
}
#doc {
  width: 100%;
  padding: 3em;
  background: white;
  color: black;
}
.profile-photo {
  width: 500px;
}
.skill-item {
  display: flex;
  align-items: center;
}
.skill-image {
  width: 200px;
  padding: 10px;
}
.uploaded {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}
.uploaded img {
  width: 200px;
  margin: 20px;
}
</style>