# Vue.JS-GalleryFileUpload
A Gallery component(s) that inherit upload/delete actions as props (allows you to use Vuex) and display images

```
<template>
   <GalleryUpload v-model="model"
      imagePath="fullPath"
      urlPrefix="http://api.awesomepage.com/"
      :allowedTypes="['image/jpeg', 'image/png']"
      :uploadAction="RECIPE_PHOTO_CREATE"
      :deleteAction="RECIPE_PHOTO_DELETE"
      @error="handleError"
      @success="handleSuccess"
      @deleted="handleDelete"/>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import GalleryUpload from './Gallery'

export default {
  data () {
    return {
      model: [
        {
          id: 1,
          fullPath: '273dvsm2y1/image1.jpg'
        },
        {
          id: 2,
          fullPath: '37c7xcma92/image2.png'
        }
      ]
    }
  },
  methods: {
    ...mapActions([
      'RECIPE_PHOTO_CREATE',
      'RECIPE_PHOTO_DELETE'
    ]),
    handleError (type, file) {
      let message
      switch (type) {
        case 'incorrectType':
          message = `Incorrect type of file '${file.name}'`
          break
        case 'incorrectSize':
          message = `File '${file.name}' is too big`
          break
        case 'failedUpload':
          message = `Uploading '${file.name}' has failed`
          break
        case 'failedDelete':
          message = `Deleting '${file.name}' has failed`
          break
      }
      this.$message.error(message)
    },
    handleSuccess (file) {
      this.$message.success(`'${file.name}' has been successfully uploaded`)
    },
    handleDelete (id) {
      this.$message.info(`File has been deleted`)
    }
  },
  components: {
    GalleryUpload
  }
}
</script>
```
