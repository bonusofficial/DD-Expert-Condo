<template>
  <div class="rich-text-editor">
    <ClientOnly>
      <QuillEditor 
        v-model:content="content" 
        contentType="html" 
        theme="snow" 
        toolbar="essential"
        class="bg-white rounded-xl overflow-hidden border border-slate-200"
      />
    </ClientOnly>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import { QuillEditor } from '@vueup/vue-quill'
import '@vueup/vue-quill/dist/vue-quill.snow.css'

const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['update:modelValue'])

const content = ref(props.modelValue)

watch(() => props.modelValue, (newValue) => {
  if (newValue !== content.value) {
    content.value = newValue
  }
})

watch(content, (newValue) => {
  emit('update:modelValue', newValue)
})
</script>

<style>
.rich-text-editor .ql-toolbar {
  border-top-left-radius: 0.75rem;
  border-top-right-radius: 0.75rem;
  border-color: #e2e8f0;
  background-color: #f8fafc;
}

.rich-text-editor .ql-container {
  border-bottom-left-radius: 0.75rem;
  border-bottom-right-radius: 0.75rem;
  border-color: #e2e8f0;
  font-family: inherit;
  font-size: 0.875rem;
}

.rich-text-editor .ql-editor {
  min-height: 200px;
}

/* Custom Active Color */
.rich-text-editor .ql-snow .ql-picker.ql-expanded .ql-picker-label {
  color: #DAA520;
}
.rich-text-editor .ql-snow .ql-picker-item:hover {
  color: #DAA520;
}
.rich-text-editor .ql-snow.ql-toolbar button:hover,
.rich-text-editor .ql-snow.ql-toolbar button:focus,
.rich-text-editor .ql-snow.ql-toolbar button.ql-active,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-label:hover,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-label.ql-active,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-item.ql-selected,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-item:hover {
  color: #DAA520;
}
.rich-text-editor .ql-snow.ql-toolbar button:hover .ql-stroke,
.rich-text-editor .ql-snow.ql-toolbar button:focus .ql-stroke,
.rich-text-editor .ql-snow.ql-toolbar button.ql-active .ql-stroke,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-label:hover .ql-stroke,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-label.ql-active .ql-stroke,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-item.ql-selected .ql-stroke,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-item:hover .ql-stroke,
.rich-text-editor .ql-snow.ql-toolbar button:hover .ql-stroke-miter,
.rich-text-editor .ql-snow.ql-toolbar button:focus .ql-stroke-miter,
.rich-text-editor .ql-snow.ql-toolbar button.ql-active .ql-stroke-miter,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-label:hover .ql-stroke-miter,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-label.ql-active .ql-stroke-miter,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-item.ql-selected .ql-stroke-miter,
.rich-text-editor .ql-snow.ql-toolbar .ql-picker-item:hover .ql-stroke-miter {
  stroke: #DAA520;
}
</style>
