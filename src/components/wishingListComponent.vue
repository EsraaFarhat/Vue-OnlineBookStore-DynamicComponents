<template>
  <div class="row">
    <h3
      class="text-danger text-center mt-5 w-100"
      v-if="listContent.length == 0"
    >
      Your whish list is Empty!
    </h3>
    <table
      v-if="listContent.length > 0"
      class="table table-striped text-center"
    >
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Price</th>
          <th>Quantity</th>
          <th>Author</th>
          <th>Category</th>
          <th>Add | Remove</th>
          <th>Total price</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in listContent" :key="item.id">
          <td>{{ item.book.id }}</td>
          <td>{{ item.book.name }}</td>
          <td>{{ currencyFormatter(item.book.price) }}</td>
          <td>{{ item.quantity }}</td>
          <td>{{ item.book.author }}</td>
          <td>{{ item.book.category }}</td>
          <td>
            <button
              class="btn btn-success"
              @click="increaseQuantityWishList(item)"
            >
              +
            </button>
            <button
              class="btn btn-danger ml-2"
              @click="decreaseQuantityWishList(item)"
            >
              -
            </button>
          </td>
          <td>{{ currencyFormatter(item.book.price * item.quantity) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
  <!-- End of Whish List -->
</template>

<script>
export default {
  props: ["listContent"],
  methods: {
    currencyFormatter(value) {
      let formatter = Intl.NumberFormat("en-Us", {
        style: "currency",
        currency: "USD",
        minimumFractionDigits: 0,
      });
      return formatter.format(value);
    },
    increaseQuantityWishList(item) {
      item.quantity++;
    },
    decreaseQuantityWishList(item) {
      item.quantity--;
      if (item.quantity == 0) {
        let itemIndex;
        for (let i = 0; i < this.listContent.length; i++) {
          if (this.listContent[i].book.id == item.book.id) {
            itemIndex = i;
            break;
          }
        }
        this.listContent.splice(itemIndex, 1);
      }
    },
  },
};
</script>
