<template>
  <div class="container">
    <div
      class="row bg-dark text-light justify-content-between p-2 align-items-baseline"
    >
      <a
        href="#"
        style="color: rgb(0, 255, 242);text-decoration: none;"
        @click.prevent="targetComponent = 'homeComponent'"
        >Books</a
      >
      <div class="d-flex align-items-baseline">
        <p class="mx-2">
          <span v-text="whishList.whishListItems.length"></span>
          <template
            v-if="
              whishList.whishListItems.length == 0 ||
                whishList.whishListItems.length > 1
            "
          >
            books</template
          ><template v-if="whishList.whishListItems.length == 1">
            book</template
          >
          in your Book WishList
        </p>
        <button
          class="btn btn-warning"
          @click.prevent="targetComponent = 'wishingListComponent'"
        >
          My WishList
        </button>
      </div>
      <div class="d-flex align-items-baseline">
        <p class="mx-2">
          <span v-text="cart.items.length"></span>
          <template v-if="cart.items.length == 0 || cart.items.length > 1">
            books</template
          >
          <template v-if="cart.items.length == 1"> book</template> with total
          price <span>{{ currencyFormatter(getTotalPrice) }}</span>
        </p>
        <button
          class="btn btn-success"
          @click.prevent="targetComponent = 'cartComponent'"
        >
          Show Cart
        </button>
      </div>
    </div>
    <!-- banner -->
      <keep-alive>
        <component col-12 :is='targetComponent' v-bind="currentProperties" @addToWishListBtnClicked="addToWishList"
        @addToChartBtnClicked="addToChart">
        </component>
      </keep-alive>
  </div>
</template>

<script>
import homeComponent from "./components/homeComponent.vue";
import cartComponent from "./components/cartComponent.vue";
import wishingListComponent from "./components/wishingListComponent.vue";
import { books } from "./books.js";
export default {
  name: "App",
  data() {
    return {
      targetComponent:'homeComponent',
      books: books,
      isCartVisible: false,
      isWhishListVisible: false,
      cart: {
        items: [],
      },
      whishList: {
        whishListItems: [],
      },
    };
  },
  methods: {
    currencyFormatter(value) {
      let formatter = Intl.NumberFormat("en-Us", {
        style: "currency",
        currency: "USD",
        minimumFractionDigits: 0,
      });
      return formatter.format(value);
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
  computed: {
    getTotalPrice() {
      let result = 0;
      for (let i = 0; i < this.cart.items.length; i++) {
        result += this.cart.items[i].book.price * this.cart.items[i].quantity;
      }
      return result;
    },
    currentProperties:function(){
      if(this.targetComponent === 'homeComponent'){
        return{
          books: this.books
        }
      }
      else if(this.targetComponent === 'cartComponent'){
        return {
          cartContent:this.cart.items
        }
      }
      else if(this.targetComponent === 'wishingListComponent'){
        return {
          listContent:this.whishList.whishListItems
        }
      }
    },
  },
  components: {
    homeComponent,
    cartComponent,
    wishingListComponent,
  },
};
</script>

