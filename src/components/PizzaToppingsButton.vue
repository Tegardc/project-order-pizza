<template>
  <label
    :class="[
      'topping-button',
      { active: activeTopping, disabled: disabledTopping },
    ]"
    :for="name"
  >
    <input
      :id="name"
      type="checkbox"
      class="visual-hidden"
      :checked="activeTopping"
      :disabled="disabledTopping"
      @change="toggleTopping"
    />{{ name }}
  </label>
</template>
<script setup>
const props = defineProps({
  name: {
    required: true,
  },
  activeTopping: {
    type: Boolean,
    default: false,
  },
  disabledTopping: {
    type: Boolean,
    default: false,
  },
});
const emit = defineEmits(["update:activeTopping"]);

const toggleTopping = (event) => {
  emit("update:activeTopping", event.target.checked);
};
</script>
<style scoped>
.topping-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 120px;
  padding: 12px;
  background-color: white;
  border-radius: 50px;
  font-weight: bold;
  color: #333333;
  border: 1px solid #33333380;
  cursor: pointer;
  text-align: center;
  transition: 0.3s;
}
.topping-button:hover {
  color: orange;
  border: 1px solid orange;
}
.topping-button.active {
  color: var(--primary);
  background-color: #e183314d;
  color: orange;
  border: 1px solid orange;
}
.topping-button[disabled],
.topping-button.disabled {
  background-color: #d8d6d6;
  color: #6c6b6b;
  border: 1px solid #dad7d7;
  cursor: not-allowed;
}
.visual-hidden {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
  pointer-events: none;
}
</style>
