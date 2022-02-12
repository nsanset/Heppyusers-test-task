<template>
  <div class="cropImage">
    <div class="title">
      <h1>Crop image component</h1>
    </div>
    <span v-if="preview" class="cropComponent">
      <vue-painting
        :img= coverUri
        @saveImage="onSaveImage"
        @copyImage="onCopyImage"
        @quit="onQuitDraw">
      </vue-painting>
    </span>
    <div class="formImageUploads">
      <form>
        <label for="my-file">Select Image: </label>
        <input type="file" accept="image/*" @change="previewImage" :key="fileInputKey" id="my-file">
        <button v-if="preview" @click="reset">Delete image</button>
      </form>
    </div>
  </div>
</template>

<script>
import VuePainting from 'vue-painting'

export default {
  name: 'cropImage',
  components: {
    VuePainting
  },
  data() {
    return {
      cropImage: '',
      preview: null,
      image: null,
      fileInputKey: 0
    }
  },
  computed: {
    // return require = (this.preview);
    coverUri() {
      return require = (this.preview);
      // if (this.cropImage) {
      //   return require = (this.preview);
      // }
      // else {
      //   return require = (this.cropImage);
      // }
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
      this.preview = null;
      var a = document.createElement('a')
      this.preview = a.href = window.URL.createObjectURL(blobFile)
    },
    onQuitDraw () {
    },
    previewImage(event) {
      var input = event.target;
      if (input.files) {
        var reader = new FileReader();
        reader.onload = (e) => {
          this.preview = e.target.result;
        }
        reader.readAsDataURL(input.files[0]);
      }
    },
    reset() {
      this.image = null;
      this.preview = null;
      this.clear()
    },
    clear() {
      this.fileInputKey++;
      if (this.image) {
        this.image = null;
      }
    }
  }
}
</script>
<style>
.cropImage {
  margin-left: 20%;
  margin-right: 20%;
  height: 600px;
}
.formImageUploads {
  margin-top: 20px;
}
</style>
