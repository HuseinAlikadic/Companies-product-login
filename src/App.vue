<template>
  <div id="app">
    <Navbar :registred_user="register_user_array" />
    <div class="container" v-show="show_content == true">
      <br />
      <ul class="nav nav-pills" role="tablist">
        <li class="nav-item">
          <a class="nav-link active" data-toggle="pill" href="#home">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" data-toggle="pill" href="#menu1">Companies</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" data-toggle="pill" href="#menu2">Products</a>
        </li>
      </ul>

      <div class="tab-content">
        <div id="home" class="container tab-pane active">
          <br />
          <h3>HOME</h3>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua,sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua.
          </p>
        </div>
        <div id="menu1" class="container tab-pane fade">
          <br />
          <h3>Companies</h3>
          <p>
            Sed ut perspiciatis unde omnis iste natus error sit voluptatem
            accusantium doloremque laudantium, totam rem aperiam sed do eiusmod
            tempor incididunt ut labore et dolore magna aliqua.
          </p>
          <Companies
            :all_companies="commpany_array"
            :companie_category="commpany_category_array"
          />
        </div>
        <div id="menu2" class="container tab-pane fade">
          <br />
          <h3>Products</h3>
          <p>
            Sed ut perspiciatis unde omnis iste natus error sit voluptatem
            accusantium doloremque laudantium, totam rem aperiam.
          </p>
          <Product
            :product_array="product_array"
            :company_name_array="commpany_array"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Companies from "./components/Companies.vue";
import Product from "./components/Product.vue";

export default {
  name: "App",
  components: {
    Navbar,
    Companies,
    Product,
  },
  data() {
    return {
      show_content: false,
      commpany_array: [
        { id: 1, name: "Vispak", adress: "Zenička", category_id: 1 },
        { id: 2, name: "Kraš", adress: "Sarajevska", category_id: 2 },
        { id: 3, name: "Zvečevo", adress: "Tuzlanska", category_id: 3 },
      ],
      commpany_category_array: [
        { id: 1, category: "Coffee" },
        { id: 2, category: "Sweets" },
        { id: 3, category: "Nutrition" },
      ],
      product_array: [
        { id: 1, commpany_id: 1, product: "Banana", price: 1.9, quantity: 8 },
        { id: 2, commpany_id: 2, product: "Apple", price: 2.7, quantity: 6 },
        { id: 3, commpany_id: 3, product: "Blum", price: 3.5, quantity: 2 },
      ],
      register_user_array: [
        { id: 1, name: "Husein", email: "husein@", password: 12345 },
        { id: 2, name: "Emina", email: "emina@", password: 12345 },
      ],
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
}
</style>
