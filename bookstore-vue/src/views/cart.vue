<template>
  <div class="cart">
    <cart-item />
  </div>
</template>
<script>
import cartItem from "@/components/cartItem.vue";

export default {
  name: "cart",
  components: {
    cartItem,
  },
};
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.cart {
  display: grid;
  justify-content: center;
  align-items: center;
  height: 80vh;
}
</style>
