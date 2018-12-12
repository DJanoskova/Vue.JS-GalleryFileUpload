<template>
  <transition-group name="gallery-fade" tag="div" class="gallery">
    <GalleryFile v-for="image in model"
      :key="image.id"
      :url="urlPrefix + image[fileName]"
      @delete="handleDelete"/>

    <div class="gallery-file gallery-file--upload"
      @click="$refs.filePicker.click()"
      :key="0">
      <i class="el-icon-plus"></i>
      <input type="file" ref="filePicker" @change="handleChange" style="display: none;" multiple>
    </div>
  </transition-group>
</template>

<script>
import GalleryFile from './GalleryFile'

export default {
  props: {
    value: {
      type: Array
    },
    fileName: {
      type: String,
      default: 'fileName'
    },
    urlPrefix: {
      type: String,
      default: ''
    },
    uploadAction: {
      type: Function
    },
    deleteAction: {
      type: Function
    },
    allowedTypes: {
      type: Array,
      default: () => ['image/gif', 'image/jpeg', 'image/png', 'image/webp']
    },
    maxSize: {
      type: Number,
      default: 2
    }
  },
  data () {
    return {
      model: this.value
    }
  },
  methods: {
    handleChange (e) {
      let input = e.target
      if (!input.files || !input.files.length) return
      for (let key in input.files) {
        if (input.files.hasOwnProperty(key)) {
          const file = input.files[key]
          if (!this.isCorrectSize(file) || !this.isCorrectType(file)) return
          this.upload(file)
        }
      }
    },
    isCorrectType (file) {
      const allowed = this.allowedTypes
      if (allowed.indexOf(file.type) < 0) {
        this.$emit('error', 'incorrectType', file)
        return false
      }
      return true
    },
    isCorrectSize (file) {
      if (file.size / 1024 / 1024 > this.maxSize) {
        this.$emit('error', 'incorrectSize', file)
        return false
      }
      return true
    },
    async upload (file) {
      const data = new FormData()
      data.append('file', file)
      try {
        const photo = await this.uploadAction(data)
        this.$emit('success', file)
        this.model.push(photo)
      } catch (_) {
        this.$emit('error', 'failedUpload', file)
      }
    },
    async handleDelete (id) {
      try {
        const photo = await this.deleteAction(id)
        this.$emit('deleted', id)
      } catch (_) {
        this.$emit('error', 'failedDelete', id)
      }
    }
  },
  components: {
    GalleryFile
  }
}
</script>
