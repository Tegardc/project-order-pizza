<template>
  <div class="pizza-toppings">
    <div v-for="topping in toppings" :key="topping.id">
      <PizzaToppingsButton
        :name="topping.name"
        :activeTopping="selectedToppings.includes(topping.id)"
        :disabledTopping="!pizzaToppings?.includes(topping.id)"
        @update:activeTopping="updateTopping(topping.id, $event)"
      />
    </div>
  </div>
</template>
<script setup>
import PizzaToppingsButton from "./PizzaToppingsButton.vue";
import { ref } from "vue";

defineProps({
  toppings: {
    type: Array,
    default: () => [],
    required: true,
  },
  pizzaToppings: {
    type: Array,
    default: () => [],
    required: true,
  },
});
const selectedToppings = ref([]);
const emit = defineEmits(["update:selectedToppings"]);

const updateTopping = (id, isActive) => {
  if (isActive) {
    selectedToppings.value.push(id);
  } else {
    selectedToppings.value = selectedToppings.value.filter(
      (topping) => topping !== id
    );
  }

  emit("update:selectedToppings", selectedToppings.value);
};
</script>
