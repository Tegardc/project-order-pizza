<template>
  <div class="pizza-sizes">
    <label
      v-for="size in sizes"
      :key="size.id"
      :class="{ disabled: !isPizzaSelected }"
    >
      <input
        :id="size.name.toLowerCase()"
        type="radio"
        name="size"
        :value="size"
        v-model="selectedSize"
        :disabled="!isPizzaSelected"
      />
      <label :for="size.name.toLowerCase()">{{ size.name }}</label>
      <span v-if="size.extra_price > 0">(+{{ size.extra_price }}$)</span>
    </label>
  </div>
</template>
<script setup>
import { ref, watch } from "vue";
const props = defineProps({
  sizes: {
    type: Array,
    required: true,
  },
  isPizzaSelected: { type: Boolean, required: true },
});
const selectedSize = ref(null);
const emit = defineEmits(["update:selectedSize"]);
watch(
  () => props.isPizzaSelected,
  (isSelected) => {
    if (isSelected && !selectedSize.value) {
      selectedSize.value = props.sizes[0];
    } else if (!isSelected) {
      selectedSize.value = null;
    }
  },
  { immediate: true }
);
watch(selectedSize, (newSize) => {
  emit("update:selectedSize", newSize);
});
</script>
<style scoped></style>
