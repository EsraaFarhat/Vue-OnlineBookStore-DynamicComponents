<template>
  <div class="row justify-content-center">
    <div
      class="card p-2"
      style="width: 23rem;margin: 0.2rem;"
      v-for="book in books"
      :key="book.id"
    >
      <img
        :src="book.image"
        style="height: 20rem;"
        class="card-img-top"
        :title="book.name"
      />
      <div class="card-body">
        <ul class="list-group list-group-flush list-unstyled">
          <li class="list-group-item">Category: {{ book.category }}</li>
          <li class="list-group-item">Author: {{ book.author }}</li>
          <li class="list-group-item">Pages: {{ book.pages }}</li>
          <li class="list-group-item">
            Price: {{ currencyFormatter(book.price) }}
          </li>
          <li
            class="list-group-item"
            :class="[
              book.inStock >= 5 ? 'more' : '',
              book.inStock < 5 ? 'less' : '',
              book.inStock == 0 ? 'none' : '',
            ]"
          >
            In Stock: {{ book.inStock }}
          </li>
          <li class="d-flex justify-content-between align-items-center">
            <button
              class="btn btn-success d-inline-block"
              @click="addToWishListBtnClicked(book)"
            >
              Add to WishList
            </button>
            <button
              class="btn btn-primary d-inline-block"
              :disabled="book.inStock == 0"
              @click="addToChartBtnClicked(book)"
            >
              Add to Cart
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
  <!-- End of books-->
</template>

<script>
export default {
  props: ["books"],
  methods: {
    currencyFormatter(value) {
      let formatter = Intl.NumberFormat("en-Us", {
        style: "currency",
        currency: "USD",
        minimumFractionDigits: 0,
      });
      return formatter.format(value);
    },
    addToWishListBtnClicked(book){
        this.$emit("addToWishListBtnClicked", book);
    },
    addToChartBtnClicked(book){
        this.$emit("addToChartBtnClicked", book);
    },
    wishListCheckExistence(book) {
      for (let i = 0; i < this.whishList.whishListItems.length; i++) {
        if (this.whishList.whishListItems[i].book.id == book.id) return true;
      }
      return false;
    },

    addToWishList(book) {
      if (!this.wishListCheckExistence(book)) {
        this.whishList.whishListItems.push({
          book: book,
          quantity: 1,
        });
      } else {
        let targetBook;
        for (let i = 0; i < this.whishList.whishListItems.length; i++) {
          if (this.whishList.whishListItems[i].book.id == book.id) {
            targetBook = this.whishList.whishListItems[i];
            break;
          }
        }
        targetBook.quantity++;
      }
    },
    addToChart(book) {
      if (!this.checkExistence(book)) {
        this.cart.items.push({
          book: book,
          quantity: 1,
        });
      } else {
        let targetItem;
        for (let i = 0; i < this.cart.items.length; i++) {
          if (this.cart.items[i].book.id == book.id) {
            targetItem = this.cart.items[i];
            break;
          }
        }
        targetItem.quantity++;
      }
      book.inStock--;
    },

    checkExistence(book) {
      for (let i = 0; i < this.cart.items.length; i++) {
        if (this.cart.items[i].book.id == book.id) return true;
      }
      return false;
    },
  },
};
</script>

<style scoped>
.more {
  color: green;
  font-weight: bold;
}
.less {
  color: orange;
  font-weight: bold;
}
.none {
  color: red;
  font-weight: bold;
}
</style>

