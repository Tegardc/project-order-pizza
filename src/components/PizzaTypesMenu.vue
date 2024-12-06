<template>
  <div class="pizza-types">
    <div v-for="type in pizzaTypes" :key="type.id">
      <PizzaTypeButton
        :pizzaSrc="`${type.name}.png`"
        :title="type.name"
        :price="`$${
          type.discount.is_active ? type.discount.final_price : type.price
        }`"
        :oldPrice="type.discount.is_active ? `$${type.price}` : ''"
        :offer="type.discount.is_active"
        :isActive="selectedPizza === type.id"
        @select="selectPizza(type)"
      />
    </div>
  </div>
</template>
<script setup>
import PizzaTypeButton from "./PizzaTypeButton.vue";
import { ref } from "vue";
defineProps({
  pizzaTypes: {
    type: Array,
    required: true,
  },
});
const emit = defineEmits(["update:selectedPizza"]);
const selectedPizza = ref(null);

const selectPizza = (pizza) => {
  selectedPizza.value = pizza.id;
  emit("update:selectedPizza", pizza);
};
</script>
<style scoped></style>
