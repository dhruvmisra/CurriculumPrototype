<template>
  <div class="generate">
    <form class="export-form row justify-content-center mx-0">
      <p class="text-center mx-3 my-0">Export as:</p>
      <div class="form-check px-3">
        <input class="form-check-input" type="radio" id="pdfRadio" value="pdf" v-model="output">
        <label class="form-check-label" for="pdfRadio">
          PDF
        </label>
      </div>
      <div class="form-check">
        <input class="form-check-input" type="radio" id="docRadio" value="doc" v-model="output">
        <label class="form-check-label" for="docRadio">
          Doc (A little buggy in styling)
        </label>
      </div>      
    </form> 

    <div class="doc-container">
      <div id="doc" class="mt-2">
        <div class="profile mx-auto" style="text-align: center">
          <h1 class="mt-5 pt-5">{{ student.name }}</h1>
          <img
            src="../assets/icons/owner-icon.svg"
            class="profile-photo d-block mx-auto"
          />
          <h5 class="my-3">Roll Number: {{ student.rollNo }}</h5>
          <h5 class="my-3">Class: My Class</h5>
          <h5 class="my-3">Age: {{ student.age }}</h5>
        </div>

        <hr class="my-5" />

        <div class="categories py-3">
          <div class="category my-5" v-for="category in categories" :key="category.id">
            <h1
              class="my-4 py-2"
              style="text-align: center"
              :style="{borderBottom: 'solid 6px ' + category.bg}"
            >{{ category.title }}</h1>
            <div class="skills">
              <div class="skill-item my-3" v-for="skill in category.skills" :key="skill.id">
                <img :src="getImage(skill.image, 'skills')" class="skill-image" />
                <div class="uploaded w-100" style="display: flex; flex-wrap: wrap; align-items: center">
                  <div class="mx-0 w-100" style="border-bottom: solid 2px grey; display:flex; width: 100%">
                    <h6>{{ skill.title }}</h6>
                    <h5
                      style="margin-left: auto"
                      :class="{ 'text-success': student.acquired[category.id][skill.id].acquired, 
                                'text-danger': !student.acquired[category.id][skill.id].acquired }"
                    >{{ student.acquired[category.id][skill.id].acquired ? 'Acquired' : 'Not Acquired' }}</h5>
                  </div>
                  <div class="uploaded" v-if="student.acquired[category.id][skill.id].image != null || student.acquired[category.id][skill.id].audio != null">
                    <img :src="student.acquired[category.id][skill.id].image" v-if="student.acquired[category.id][skill.id].image != null">
                    <div v-if="student.acquired[category.id][skill.id].audio != null" class="m-5">[Audio added]</div>
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
    student: Object,
    categories: Array
  },
  data() {
    return {
      selectedStudent: 0,
      output: 'pdf'
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
    generate() {
      if(this.output == 'pdf') {
        this.generatePDF();
      } else {
        this.generateDoc();
      }
    },
    generateDoc() {
      let vm = this;
      let filename = "skills-" + this.student.name;
      let element = "doc";

      setTimeout(() => {
        var preHtml = "<html xmlns:o='urn:schemas-microsoft-com:office:office' xmlns:w='urn:schemas-microsoft-com:office:word' xmlns='http://www.w3.org/TR/REC-html40'><head><meta charset='utf-8'><title>Export HTML To Doc</title></head><body>";
        var postHtml = "</body></html>";
        var html = preHtml+document.getElementById(element).innerHTML+postHtml;
  
        var blob = new Blob(['\ufeff', html], {
            type: 'application/msword'
        });
        
        // Specify link url
        var url = 'data:application/vnd.ms-word;charset=utf-8,' + encodeURIComponent(html);
        filename = filename?filename+'.doc':'document.doc';
        
        // Create download link element
        var downloadLink = document.createElement("a");
        document.body.appendChild(downloadLink);
        if(navigator.msSaveOrOpenBlob ){
            navigator.msSaveOrOpenBlob(blob, filename);
        }else{
            // Create a link to the file
            downloadLink.href = url;
            // Setting the file name
            downloadLink.download = filename;
            //triggering the function
            downloadLink.click();
        }
        document.body.removeChild(downloadLink);
      }, 500);
    },
    generatePDF() {
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
          
          pdf.save("Skills-" + vm.student.name + ".pdf");
        });
      }, 500);
    }
  }
};
</script>

<style>
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
.export-form {
  position: absolute;
  bottom: 0;
  left: 0;
  background: white;
}
</style>