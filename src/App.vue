<script setup>
import NavBar from "./components/NavBar.vue";
import HeroSection from "./components/HeroSection.vue";
import FooterSection from "./components/FooterSection.vue";

import sizes from "./assets/data/size-list.json";
import toppings from "./assets/data/topping-list.json";
import pizzaList from "./assets/data/pizza-list.json";
import PizzaTypesMenu from "./components/PizzaTypesMenu.vue";
import PaymentMenu from "./components/PaymentMenu.vue";
import PizzaSizesMenu from "./components/PizzaSizesMenu.vue";
import PizzaToppingsMenu from "./components/PizzaToppingsMenu.vue";
import { ref, computed } from "vue";

const selectedPizza = ref(null);
const selectedSize = ref(null);
const selectedToppings = ref([]);
const currentAvailableToppings = ref([]);

const handlePizzaSelection = (pizza) => {
  selectedPizza.value = pizza;
  currentAvailableToppings.value = pizza.availableToppings || [];
};
const handleSizeChange = (size) => {
  selectedSize.value = size;
};
const handleToppingChange = (toppings) => {
  selectedToppings.value = toppings;
};

const isPizzaSelected = computed(() => selectedPizza.value !== null);

const updateSelectedToppings = (newToppings) => {
  selectedToppings.value = newToppings;
};

const resetAllData = () => {
  selectedPizza.value = null;
  selectedSize.value = null;
  selectedToppings.value = [];
  console.log("Order data has been reset!");
};
</script>

<template>
  <header>
    <NavBar />
  </header>
  <HeroSection />

  <main>
    <section class="pizza-options">
      <div>
        <h2>Choose Your Pizza</h2>
        <PizzaTypesMenu
          :pizzaTypes="pizzaList.data"
          @update:selectedPizza="handlePizzaSelection"
        />
      </div>
      <div>
        <h2>Custom Pizza</h2>
        <h3>Size</h3>
        <PizzaSizesMenu
          :sizes="sizes.data"
          :isPizzaSelected="isPizzaSelected"
          @update:selectedSize="handleSizeChange"
        />
      </div>
      <div>
        <h3>Toppings</h3>
        <PizzaToppingsMenu
          :toppings="toppings.data"
          :pizzaToppings="selectedPizza?.toppings || []"
          @update:selectedToppings="handleToppingChange"
        />
      </div>
    </section>
    <aside>
      <PaymentMenu
        :selectedPizza="selectedPizza"
        :selectedSize="selectedSize"
        :selectedToppings="selectedToppings"
        :toppingsList="toppings.data"
        @update:selectedToppings="updateSelectedToppings"
        @orderReset="resetAllData"
      />
    </aside>
  </main>
  <FooterSection />
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
