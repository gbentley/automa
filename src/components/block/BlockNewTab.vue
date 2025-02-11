<template>
  <div :id="componentId" class="p-4">
    <div class="flex items-center mb-2">
      <div
        :class="block.category.color"
        class="inline-block text-sm mr-4 p-2 rounded-lg"
      >
        <v-remixicon name="riGlobalLine" size="20" class="inline-block mr-1" />
        <span>{{ block.details.name }}</span>
      </div>
      <div class="flex-grow"></div>
      <v-remixicon
        name="riDeleteBin7Line"
        class="cursor-pointer"
        @click="editor.removeNodeId(`node-${block.id}`)"
      />
    </div>
    <input
      :value="block.data.url"
      class="px-4 py-2 mb-1 rounded-lg block w-48 bg-input"
      placeholder="http://example.com"
      type="url"
      required
      @blur="checkInputValue"
      @input="handleInput"
    />
    <ui-checkbox :model-value="block.data.active" @change="handleCheckbox">
      Set as active tab
    </ui-checkbox>
  </div>
</template>
<script setup>
import emitter from 'tiny-emitter/instance';
import { debounce } from '@/utils/helper';
import { useComponentId } from '@/composable/componentId';
import { useEditorBlock } from '@/composable/editorBlock';

const props = defineProps({
  editor: {
    type: Object,
    default: () => ({}),
  },
});

const componentId = useComponentId('new-tab');
const block = useEditorBlock(`#${componentId}`, props.editor);

const isValidURL = (url) => /^(https?):\/\//i.test(url);
const handleInput = debounce(({ target }) => {
  target.reportValidity();

  block.data.url = isValidURL(target.value) ? target.value : '';

  props.editor.updateNodeDataFromId(block.id, {
    ...block.data,
    url: block.data.url,
  });
  emitter.emit('editor:data-changed', block.id);
}, 250);
function handleCheckbox(value) {
  props.editor.updateNodeDataFromId(block.id, { ...block.data, active: value });
  block.data.active = value;
  emitter.emit('editor:data-changed', block.id);
}
function checkInputValue({ target }) {
  if (!isValidURL(target.value)) {
    target.value = '';
  }
}
</script>
