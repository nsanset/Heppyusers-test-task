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
      preview: null,
      image: null,
      _list: [],
      image_list: [],
      fileInputKey: 0
    }
  },
  computed: {
    coverUri() {
      return require = (this.preview);
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
      var a = document.createElement('a')
      this.preview = a.href = window.URL.createObjectURL(blobFile)
    },
    onQuitDraw (a) {

    },
    previewImage(event) {
      var input = event.target;
      if (input.files) {
        var reader = new FileReader();
        reader.onload = (e) => {
          this.preview = e.target.result;
        }
        this.image=input.files[0];
        reader.readAsDataURL(input.files[0]);
      }
    },
    reset() {
      this.image = null;
      this.preview = null;
      this.image_list = [];
      this.preview_list = [];
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
