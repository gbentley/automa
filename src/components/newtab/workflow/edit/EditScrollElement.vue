<template>
  <edit-interaction-base v-bind="{ data }" @change="updateData">
    <div v-if="!data.scrollIntoView" class="flex items-center mt-3 space-x-2">
      <ui-input
        :model-value="data.scrollX || 0"
        type="number"
        label="Scroll horizontal"
        @change="updateData({ scrollX: +$event })"
      />
      <ui-input
        :model-value="data.scrollY || 0"
        type="number"
        label="Scroll vertical"
        @change="updateData({ scrollY: +$event })"
      />
    </div>
    <div class="mt-3 space-y-2">
      <ui-checkbox
        class="w-full"
        :model-value="data.scrollIntoView"
        @change="updateData({ scrollIntoView: $event })"
      >
        Scroll into view
      </ui-checkbox>
      <ui-checkbox
        :model-value="data.smooth"
        @change="updateData({ smooth: $event })"
      >
        Smooth scroll
      </ui-checkbox>
      <template v-if="!data.scrollIntoView">
        <ui-checkbox
          :model-value="data.incX"
          @change="updateData({ incX: $event })"
        >
          Increment horizontal scroll
        </ui-checkbox>
        <ui-checkbox
          :model-value="data.incY"
          @change="updateData({ incY: $event })"
        >
          Increment vertical scroll
        </ui-checkbox>
      </template>
    </div>
  </edit-interaction-base>
</template>
<script setup>
import EditInteractionBase from './EditInteractionBase.vue';

const props = defineProps({
  data: {
    type: Object,
    default: () => ({}),
  },
});
const emit = defineEmits(['update:data']);

function updateData(value) {
  emit('update:data', { ...props.data, ...value });
}
</script>
