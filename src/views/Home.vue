<template>
  <div class="home">
    <cropImage/>
  </div>
</template>

<script>
// @ is an alias to /src
import cropImage from '@/components/cropImage.vue'

export default {
  name: 'Home',
  components: {
    cropImage
  }
}
</script>
