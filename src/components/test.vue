<template>
  	<div class="cropImage">
  		<div class="title">
	      	<h2>Crop image component</h2>
	    </div>
		<div class="imageUploads">
			<span>Select Image: </span>			
			<img :src="getImg"/>
		    <input type="file" accept="image/*" @change="onFileChange" :key="fileInputKey">
		</div>
		<span v-if="image">
			<vue-painting
		        :img= coverUri
		        @saveImage="onSaveImage"
		        @copyImage="onCopyImage"
		        @quit="onQuitDraw">
		    </vue-painting>
		</span>
		<div class="buttons">
		    <button v-if="image" @click="deleteImage">Delete image</button>
		    <button v-if="image" @click="downloadImage">Download image</button>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
import VueAxios from 'vue-axios'
import VuePainting from 'vue-painting'
export default {
	name: 'cropImage',
	components: {
		VuePainting
	},
	data() {
		return {
			image: '',
			fileInputKey: 0,
	    }
	},
    computed: {
   		getImg() {
   			var vm = this
    		vm.get('img')
   		},
	    coverUri() {
	    	return require = (this.image);
	    }
    },
	methods: {
	 	onSaveImage (blobFile) {
		    var a = document.createElement('a')
		    a.download = `painting-${+new Date}.png`
		    a.href = window.URL.createObjectURL(blobFile)
		    a.click()
	    },
	    onCopyImage (blobFile) {
			this.createImage(blobFile);
			this.$router.go(this.$router.currentRoute)
	    },
	    onQuitDraw (a) {
	    },
	    get(key) {
	    	this.image=localStorage.getItem(key);
	    },
	    set(key) {
	    	var vm = this
	    	try{
	   			localStorage.setItem(key,this.image);
	    	}
	    	catch(e) {
	    		console.log( `Storage failed: ${e}`);
	    	}
	    },
	    onFileChange(e) {
	    	var files = e.target.files || e.dataTransfer.files;
	    	if (!files.length)
	    	return;
	    	this.createImage(files[0]);
	    },
	    createImage(file) {
	    	var image = new Image();
	   		var reader = new FileReader();
	    	var vm = this;
	      
	     	reader.onload = (e) => {
	        	vm.image = e.target.result;
	        	vm.set('img');
	      	};
	      	reader.readAsDataURL(file);
	    },
	    downloadImage() {
              axios({
                    url: this.image,
                    method: 'GET',
                    responseType: 'blob',
                }).then((response) => {
                     var fileURL = window.URL.createObjectURL(new Blob([response.data]));
                     var fileLink = document.createElement('a');
   
                     fileLink.href = fileURL;
                     fileLink.setAttribute('download', 'img.jpg');
                     document.body.appendChild(fileLink);
   
                     fileLink.click();
                });
          },
	    deleteImage() {
	    	this.image = '';
	    	this.fileInputKey++;
	    	localStorage.removeItem('img');
	    }
  	}
}
</script>
<style>
.cropImage {
  	margin-left: 25%;
  	margin-right: 25%;
  	height: 50vh;
}
.title {
	text-align: center;
}
.imageUploads {
	margin-bottom: 15px;
	text-align: center;
}
.btnDelete {
	margin-top: 15px;
}
.buttons > button {
	padding: 10px;
	margin: 5px;
	cursor: pointer;
	text-align: left;
}
.abandon {
	display: none !important;
}
</style>
