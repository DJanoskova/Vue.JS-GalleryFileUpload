<template>
  <div class="gallery-file">
    <img :src="url"/>
    <div class="gallery-file__overlay">
      <i class="el-icon-delete" @click="$emit('detete', id)"></i>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    id: {
      type: String | Number
    },
    url: {
      type: String
    }
  }
}
</script>
