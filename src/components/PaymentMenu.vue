<template>
  <div>
    <h2>Payment Summary</h2>
    <div v-if="!selectedPizza">
      <p class="empty-message">Choose Pizza!!!</p>
    </div>
    <table v-else>
      <tbody>
        <tr v-if="selectedPizza">
          <td width="50%">{{ selectedPizza?.name }}</td>
          <td width="50%">{{ formatCurrency(selectedPizza?.price) }}</td>
        </tr>
        <tr v-if="selectedSize">
          <td width="50%">Size-{{ selectedSize?.name }}</td>
          <td width="50%">{{ formatCurrency(selectedSize?.extra_price) }}</td>
        </tr>
        <tr v-for="topping in enrichedToppings" :key="topping.id">
          <td width="50%">{{ topping.name }}</td>
          <td width="50%">
            {{ formatCurrency(topping.price) }}
          </td>
        </tr>
      </tbody>
    </table>
    <div class="count-total">
      <hr class="divider" />
      <div>
        <span>Total Price</span>
        <span class="total-price">
          <span style="color: orange">{{
            formatCurrency(totalPrice)
          }}</span></span
        >
      </div>
      <button
        type="button"
        class="btn btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#exampleModal"
        :disabled="!selectedPizza"
        @click="resetOrder"
      >
        Order Now
      </button>
    </div>
  </div>
  <div
    class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
    data-bs-backdrop="false"
  >
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header justify-content-center">
          <h1 class="modal-title fs-5" id="exampleModalLabel">
            <img src="@/assets/images/modal.png " />
          </h1>
        </div>
        <div class="modal-body">
          <h2>
            <span style="color: black"><b>Order Success</b></span>
          </h2>
          <p>Thank you, we have received your</p>
          <p>order successfully</p>
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { computed, watch } from "vue";

const props = defineProps({
  selectedPizza: {
    type: Object,
    default: null,
  },
  selectedSize: {
    type: Object,
    default: null,
  },
  selectedToppings: {
    type: Array,
    default: () => [],
  },
  toppingsList: {
    type: Array,
    required: true,
  },
});
const emit = defineEmits(["update:selectedToppings", "orderReset"]);

const formatCurrency = (value) => `$${(value || 0).toFixed(2)}`;

const enrichedToppings = computed(() =>
  props.selectedToppings.map((id) =>
    props.toppingsList.find((topping) => topping.id === id)
  )
);

const totalPrice = computed(() => {
  const pizzaPrice = props.selectedPizza?.price || 0;
  const sizePrice = props.selectedSize?.extra_price || 0;
  const toppingsPrice = enrichedToppings.value.reduce(
    (sum, topping) => sum + (topping?.price || 0),
    0
  );

  return pizzaPrice + sizePrice + toppingsPrice;
});
const resetOrder = () => {
  emit("orderReset");
};

watch(
  () => props.selectedPizza,
  () => {
    emit("update:selectedToppings", []);
  }
);
</script>

<style scoped>
.modal-header {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.modal-image {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 0 auto;
}
.modal-body {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 0 auto;
  text-align: center;
  padding: 0%;
  gap: -10px;
}
.modal-body h2 {
  margin-bottom: 10px;
}

.modal-body p {
  margin: 5px 0;
}
.modal-dialog {
  max-width: 400px;
  margin: 20px auto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal-backdrop {
  backdrop-filter: blur(10px);
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-footer {
  display: flex;
  justify-content: center;
  gap: 20px;
}

.btn:hover {
  background-color: #d26f1e;
}
.btn {
  width: 100%;
  padding: 14px;
  border-radius: 30px;
  border: none;
  background-color: orange;
  font-weight: bold;
  color: #fff;
}
.empty-message {
  color: gray;
  font-size: 1.2rem;
  text-align: center;
  margin-top: 20px;
}
</style>
