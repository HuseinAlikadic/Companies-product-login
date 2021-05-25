<template>
  <div>
    <button
      type="button"
      class="btn btn-primary float-right"
      @click="open_modal_to_add_new_product()"
    >
      Add product
    </button>
    <br />
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Num</th>
          <th>Company</th>
          <th>Product</th>
          <th>Price</th>
          <th>Quantity</th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, index) in show_product_array"
          :key="'product' + index"
        >
          <td>{{ index + 1 }}</td>
          <td>{{ item.company_name }}</td>
          <td>{{ item.product }}</td>
          <td>{{ item.price }} KM</td>
          <td>{{ item.quantity }} kom</td>
          <td>
            <i
              class="fas fa-edit"
              title="Edit"
              @click="show_modal_for_edit_product(index)"
            ></i>
          </td>
          <td>
            <i
              class="fas fa-trash-alt"
              title="Delete"
              @click="show_delete_message_modal(index)"
            ></i>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- Modal to confirme delete product -->
    <modal name="confirmeDeleteProductMessage" :adaptive="true" height="auto">
      <!-- Modal Header -->
      <div class="modal-header">
        <h4 class="modal-title">Modal Heading</h4>
        <button
          type="button"
          class="close"
          @click="$modal.hide('confirmeDeleteProductMessage')"
        >
          &times;
        </button>
      </div>

      <!-- Modal body -->
      <div class="modal-body">
        <p>
          Are you sure you want to delete company
          <strong> {{ company_name_for_delete }} </strong> and product
          <strong>{{ product_name_for_delete }}</strong> !!!
        </p>
      </div>

      <!-- Modal footer -->
      <div class="modal-footer">
        <button
          type="button"
          class="btn btn-danger"
          @click="delete_product_from_array()"
        >
          Delete
        </button>
        <button
          type="button"
          class="btn btn-primary"
          @click="$modal.hide('confirmeDeleteProductMessage')"
        >
          Close
        </button>
      </div>
    </modal>
    <!-- Modal to edit product -->
    <modal name="editProductModal" :adaptive="true" height="auto">
      <!-- Modal Header -->
      <div class="modal-header">
        <h4 class="modal-title">Edit product:</h4>
        <button
          type="button"
          class="close"
          @click="$modal.hide('editProductModal')"
        >
          &times;
        </button>
      </div>

      <!-- Modal body -->
      <div class="modal-body">
        <div class="form-group">
          <label for="sel1">Select list:</label>
          <select class="form-control" id="sel1" v-model="edit_company_name">
            <option
              v-for="(item, index) in company_name_array"
              :key="'modalProduct' + index"
              :value="item.id"
              >{{ item.name }}</option
            >
          </select>
        </div>
        <div class="form-group">
          <label for="usr">Product:</label>
          <input type="text" class="form-control" v-model="edit_product_name" />
        </div>
        <div class="form-group">
          <label for="usr">Price:</label>
          <input
            type="number"
            class="form-control"
            v-model="edit_product_price"
          />
        </div>
        <div class="form-group">
          <label for="usr">Quantity:</label>
          <input
            type="number"
            class="form-control"
            v-model="edit_product_quantity"
          />
        </div>
      </div>

      <!-- Modal footer -->
      <div class="modal-footer">
        <button
          type="button"
          class="btn btn-primary"
          @click="save_change_in_table()"
        >
          Save
        </button>
      </div>
    </modal>
    <!-- Modal to add new product -->
    <modal name="addNewProduct" :adaptive="true" height="auto">
      <!-- Modal Header -->
      <div class="modal-header">
        <h4 class="modal-title">Edit product:</h4>
        <button
          type="button"
          class="close"
          @click="$modal.hide('addNewProduct')"
        >
          &times;
        </button>
      </div>

      <!-- Modal body -->
      <div class="modal-body">
        <div class="form-group">
          <label for="sel1">Select list:</label>
          <select class="form-control" id="sel1" v-model="add_new_company">
            <option
              v-for="(item, index) in company_name_array"
              :key="'modalProduct' + index"
              :value="item.id"
              >{{ item.name }}</option
            >
          </select>
        </div>
        <div class="form-group">
          <label for="usr">Product:</label>
          <input type="text" class="form-control" v-model="add_new_product" />
        </div>
        <div class="form-group">
          <label for="usr">Price:</label>
          <input
            type="number"
            class="form-control"
            v-model="edit_product_price"
          />
        </div>
        <div class="form-group">
          <label for="usr">Quantity:</label>
          <input
            type="number"
            class="form-control"
            v-model="add_new_quantity"
          />
        </div>
        <p class="text-danger" v-show="message_for_add_product_error == true">
          You need to fill in all the fields !!!
        </p>
      </div>

      <!-- Modal footer -->
      <div class="modal-footer">
        <button
          type="button"
          class="btn btn-primary"
          @click="add_and_save_new_product()"
        >
          Save
        </button>
      </div>
    </modal>
  </div>
</template>
<script>
export default {
  name: "Products",
  props: {
    product_array: Array,
    company_name_array: Array,
  },
  data() {
    return {
      show_product_array: [],
      show_modal_for_confirme_delete: false,
      index_product: "",
      company_name_for_delete: "",
      product_name_for_delete: "",
      edit_company_name: "",
      edit_product_name: "",
      edit_product_price: "",
      edit_product_quantity: "",
      add_new_company: "",
      add_new_product: "",
      add_new_price: "",
      add_new_quantity: "",
      message_for_add_product_error: false,
    };
  },
  mounted() {
    this.show_product();
  },
  methods: {
    show_product() {
      this.show_product_array = [];
      this.product_array.forEach((item) => {
        let index_company = this.company_name_array.findIndex(
          (a) => a.id == item.commpany_id
        );
        let name_of_company = this.company_name_array[index_company].name;
        this.show_product_array.push({
          id: item.id,
          company_name: name_of_company,
          company_id: item.commpany_id,
          product: item.product,
          price: item.price,
          quantity: item.quantity,
        });
      });
    },
    show_delete_message_modal(h) {
      this.index_product = h;
      this.show_modal_for_confirme_delete = true;
      this.$modal.show("confirmeDeleteProductMessage");
      let company_neme = this.show_product_array[h].company_name;
      this.company_name_for_delete = company_neme;

      let product_name = this.show_product_array[h].product;
      this.product_name_for_delete = product_name;
    },
    delete_product_from_array() {
      this.$modal.hide("confirmeDeleteProductMessage");
      this.product_array.splice(this.index_product, 1);

      this.show_product();
    },
    show_modal_for_edit_product(h) {
      this.index_product = h;
      this.$modal.show("editProductModal");
      this.edit_company_name = this.show_product_array[h].company_id;
      this.edit_product_name = this.show_product_array[h].product;
      this.edit_product_price = this.show_product_array[h].price;
      this.edit_product_quantity = this.show_product_array[h].quantity;
    },
    save_change_in_table() {
      let b = this.index_product;
      this.product_array[b].commpany_id = this.edit_company_name;
      this.product_array[b].product = this.edit_product_name;
      this.product_array[b].price = this.edit_product_price;
      this.product_array[b].quantity = this.edit_product_quantity;
      this.show_product();
      this.$modal.hide("editProductModal");
    },
    open_modal_to_add_new_product() {
      this.$modal.show("addNewProduct");
    },
    add_and_save_new_product() {
      let max_id = Math.max.apply(
        Math,
        this.product_array.map(function(o) {
          return o.id;
        })
      );
      if (
        this.add_new_company == "" ||
        this.add_new_product == "" ||
        this.edit_product_price == "" ||
        this.add_new_quantity == ""
      ) {
        this.message_for_add_product_error = true;
      } else {
        this.product_array.push({
          id: max_id + 1,
          commpany_id: this.add_new_company,
          product: this.add_new_product,
          price: this.edit_product_price,
          quantity: this.add_new_quantity,
        });
        this.message_for_add_product_error = false;
        this.show_product();
        this.$modal.hide("addNewProduct");

        (this.add_new_company = ""),
          (this.add_new_product = ""),
          (this.edit_product_price = ""),
          (this.add_new_quantity = "");
      }
    },
  },
};
</script>
<style scoped>
.fa-edit {
  cursor: pointer;
}
.fa-trash-alt {
  cursor: pointer;
}
</style>
