<template>
  <div class="max-w-screen-2xl mx-auto grid grid-cols-2 gap-10 h-screen">
    <div class="h-full p-10 family-poppins">
      <h1 class="text-3xl font-semibold mb-8">Create your own Post</h1>
      <div class="grid grid-cols-2 gap-6">
        <div>
          <label for="name">Enter your Name</label>
          <input type="text" v-model="name" class="w-full border border-gray-300 rounded-lg px-4 py-2.5 text-base font-medium focus:outline-none">
        </div>
        <div>
          <label for="name">Enter your username</label>
          <input type="text" v-model="username" class="w-full border border-gray-300 rounded-lg px-4 py-2.5 text-base font-medium focus:outline-none">
        </div>
        <div>
          <label for="name">Select your profile pic</label>
          <input ref="fileInput" @input="pickFile" type="file" class="w-full border border-gray-300 rounded-lg px-4 py-2 text-base font-medium focus:outline-none">
        </div>
        <div class="relative">
          <label for="name">Select Font Family</label>
          <div @click="familyDropdown = !familyDropdown" class="w-full border border-gray-300 rounded-lg px-4 py-2.5 text-base font-medium focus:outline-none cursor-pointer relative">
            {{message}}
          </div>
          <div class="w-full absolute top-[4.4rem] bg-gray-800 z-50">
            <div v-for="(item, index) in Family" v-if="familyDropdown">
              <div @click="selectFamily(item)" class="flex cursor-pointer items-center px-3 py-2.5 border-b text-sm font-medium text-white">
                {{item.family_name}}
              </div>
            </div>  
          </div>
        </div>
        <div class="relative">
          <label for="name">Select Social Media Name</label>
          <div @click="imageDropdown = !imageDropdown" class="w-full border border-gray-300 rounded-lg px-4 py-2.5 text-base font-medium focus:outline-none cursor-pointer relative">
            {{image_message}}
          </div>
          <div class="w-full absolute top-[4.4rem] bg-gray-800 z-50">
            <div v-for="(item, index) in SocialImages" v-if="imageDropdown">
              <div @click="selectSocialImage(item)" class="flex cursor-pointer items-center px-3 py-2.5 border-b text-sm font-medium text-white">
                {{item.image_name}}
              </div>
            </div>
              
          </div>
        </div>
        <div class="col-span-2">
          <label for="name">Enter Your Quote</label>
          <editor v-model="content" class="h-32"></editor>
        </div>
      </div>
    </div>
    <div class="bg-blue-500 h-full flex items-center justify-center relative">
      <div ref="printcontent">
          <div :class="'bg-white rounded-md p-8 w-96 h-96 ' +selectedFamily">
          <div class="flex">
            <div>
              <img class="rounded-full" :src="previewImage" width="75" height="75" alt="">
            </div>
            <div class="ml-2.5">
              <div class="flex">
                <h2 class="text-xl font-semibold text-gray-900">{{name}}</h2>
                <img class="ml-2.5" :src="selected_social_media" width="28" height="28" alt="">
              </div>
              <p class="text-base font-medium text-gray-700">{{username}}</p>
            </div>
          </div>
          <div class="mt-6">
            <div v-html="content"></div>
          </div>
        </div>
      </div>
      <div class="absolute bottom-40 left-0 w-full flex justify-center family-poppins">
        <button @click.prevent="printThis" class="bg-white rounded-md px-6 py-2.5 text-blue-600 text-base font-medium">Download</button>
      </div>
    </div>
    
  </div>
</template>

<script>
import editor from '@tinymce/tinymce-vue';
import UserImage from './components/UserImage.vue'
export default {
    name: "AddCampaign",
    components: {editor, UserImage},
    data() {
      return {
        previewImage: 'src/assets/unknown.webp',
        content:[''],
        images:'',
        name:'',
        familyDropdown: false,
        imageDropdown: false,
        message:'Select your Family',
        image_message:'Select your Social Media Icon',
        selected_social_media:'src/assets/icons/linkedin.png',
        username:'',
        desc:'',
        selectedFamily:'',
        html2canvas:'',
        scale:2,
        Family:[
          {family_name:'Poppins', family_value:'family-poppins'},
          {family_name:'Inter', family_value:'family-inter'},
        ],
        SocialImages:[
          {image_name:'Facebook', image_value:'src/assets/icons/facebook.png'},
          {image_name:'LinkedIn', image_value:'src/assets/icons/linkedin.png'},
          {image_name:'Instagram', image_value:'src/assets/icons/instagram.png'},
          {image_name:'Twitter', image_value:'src/assets/icons/twitter.png'},
        ]
      }
    },
    methods:{
      selectFamily(item){
        this.message = item.family_name
        this.selectedFamily = item.family_value
        this.familyDropdown = false
      },
      selectSocialImage(item){
        this.image_message = item.image_name
        this.selected_social_media = item.image_value
        this.imageDropdown = false
      },
      selectImage () {
          this.$refs.fileInput.click()
      },
      pickFile () {
        let input = this.$refs.fileInput
        let file = input.files
        if (file && file[0]) {
          let reader = new FileReader
          reader.onload = e => {
            this.previewImage = e.target.result
          }
          reader.readAsDataURL(file[0])
        //   this.$emit('input', file[0])
        }
      },
      async printThis() {
        console.log("printing..");
        const el = this.$refs.printcontent;

        const options = {
          type: "dataURL",
        };
        const printCanvas = await html2canvas(el, options);

        // canvas = document.createElement('canvas');
        // ctx = canvas.getContext('2d');
        // canvas.width = 200*scale;
        // canvas.height = 200*scale;
        // tempImg = document.createElement('img');

        const link = document.createElement("a");
        // const ctx = link.getContext('2d');
        link.width = 200 * 100;
        link.height = 200 * 100;
        link.setAttribute("download", "download.png");
        link.setAttribute("href", printCanvas
            .toDataURL("image/png")
            .replace("image/png")
        );
        link.click();
        console.log("done");
      },
    }
}
</script>


<style>

</style>
