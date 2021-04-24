<template>
  <div class="row">
    <h3
      class="text-danger text-center mt-5 w-100"
      v-if="cartContent.length == 0"
    >
      Your cart is Empty!
    </h3>
    <table
      v-if="cartContent.length > 0"
      class="table table-striped text-center"
    >
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Price</th>
          <th>Quantity</th>
          <th>Add | Remove</th>
          <th>Total price</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in cartContent" :key="item.id">
          <td>{{ item.book.id }}</td>
          <td>{{ item.book.name }}</td>
          <td>{{ currencyFormatter(item.book.price) }}</td>
          <td>{{ item.quantity }}</td>
          <td>
            <button
              class="btn btn-success"
              @click="increaseQuantityCart(item)"
              :disabled="item.book.inStock == 0"
            >
              +
            </button>
            <button
              class="btn btn-danger ml-2"
              @click="decreaseQuantityCart(item)"
            >
              -
            </button>
          </td>
          <td>{{ currencyFormatter(item.book.price * item.quantity) }}</td>
        </tr>
        <tr>
          <th colspan="4">Total</th>
          <th colspan="2">{{ currencyFormatter(getTotalPrice) }}</th>
        </tr>
        <tr>
          <th colspan="4">Taxes</th>
          <th colspan="2">{{ currencyFormatter(getTotalPrice * 0.1) }}</th>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <th colspan="4">Grand Total</th>
          <th colspan="2">
            {{ currencyFormatter(getTotalPrice + getTotalPrice * 0.1) }}
          </th>
        </tr>
      </tfoot>
    </table>
  </div>
  <!-- End of cart -->
</template>

<script>
export default {
  props: ["cartContent"],
  methods: {
    currencyFormatter(value) {
      let formatter = Intl.NumberFormat("en-Us", {
        style: "currency",
        currency: "USD",
        minimumFractionDigits: 0,
      });
      return formatter.format(value);
    },
    increaseQuantityCart(item) {
      item.quantity++;
      item.book.inStock--;
    },

    decreaseQuantityCart(item) {
      item.quantity--;
      if (item.quantity == 0) {
        let itemIndex;
        for (let i = 0; i < this.cartContent.length; i++) {
          if (this.cartContent[i].book.id == item.book.id) {
            itemIndex = i;
            break;
          }
        }
        this.cartContent.splice(itemIndex, 1);
      }
      item.book.inStock++;
    },
  },
  computed: {
    getTotalPrice() {
      let result = 0;
      for (let i = 0; i < this.cartContent.length; i++) {
        result += this.cartContent[i].book.price * this.cartContent[i].quantity;
      }
      return result;
    },
  },
};
</script>
