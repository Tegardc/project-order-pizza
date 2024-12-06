<template>
  <label :class="buttonClass" :for="title" class="pizza-types-button">
    <input
      :id="title"
      type="radio"
      name="pizza"
      class="visual-hidden"
      :value="title"
      :checked="isActive"
      @change="selectPizza"
    />
    <div class="pizza-img">
      <img :src="getImageUrl(pizzaSrc)" alt="Pizza Image" />
    </div>
    <div class="pizza-price">
      <h3>{{ title }}</h3>
      <span>{{ price }}</span>
      <span v-if="oldPrice" class="price-disabled">{{ oldPrice }}</span>
    </div>
    <img
      v-if="offer"
      class="ribbon-img"
      src="../assets/images/ribbon.svg"
      alt="Special Offer"
    />
  </label>
</template>
<script setup>
import { computed } from "vue";
const getImageUrl = (name) => {
  return new URL(`../assets/images/pizza/${name}`, import.meta.url).href;
};
const props = defineProps({
  pizzaSrc: { required: true },
  title: {
    required: true,
  },
  price: {
    required: true,
  },
  oldPrice: {
    default: null,
  },
  offer: {
    default: false,
  },
  isActive: { default: false },
});
const emit = defineEmits(["select"]);
const selectPizza = () => {
  console.log("Pizza selected:", props.title);
  emit("select");
};
const buttonClass = computed(() => ({
  active: props.isActive,
}));
</script>
<style scoped>
.pizza-types-button {
  display: flex;
  align-items: center;
  border-radius: 16px;
  border: 1px solid #3333331a;
  background: white;
  cursor: pointer;
  transition: 0.3s;
  padding: 10px;
  flex-grow: 1;
  position: relative;
}
.pizza-types-button:hover {
  background-color: #e77e234d;
}

.pizza-types-button.active {
  background-color: orange;
  color: rgb(255, 255, 255);
}

.pizza-img img {
  width: 100%;
  transition: 0.3s;
}

.pizza-types-button:hover .pizza-img img {
  transform: rotate(10deg);
}

.visual-hidden {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
  pointer-events: none;
}

.pizza-price {
  flex-basis: 50%;
  text-align: left;
  font-size: 0.9rem;
}

.pizza-price .price-disabled {
  margin-left: 5px;
  text-decoration: line-through;
  opacity: 0.5;
}

.ribbon-img {
  position: absolute;
  top: 0;
  right: 0;
  width: 40%;
}
</style>
