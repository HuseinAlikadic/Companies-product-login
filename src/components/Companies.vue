<template>
  <div>
    <button
      type="button"
      class="btn btn-primary float-right"
      @click="$modal.show('modalToAddCompany')"
    >
      Add Company
    </button>
    <br />
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Num.</th>
          <th>Companies</th>
          <th>Category</th>
          <th>City</th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, index) in company_array_with_all_data"
          :key="'company' + index"
        >
          <td>{{ index + 1 }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.category }}</td>
          <td>{{ item.adress }}</td>
          <td>
            <i
              class="fas fa-edit"
              title="Edit"
              @click="show_data_to_edit(index)"
            ></i>
          </td>
          <td>
            <i
              class="fas fa-trash-alt"
              title="Delete"
              data-toggle="modal"
              data-target="#deleteCompanies"
              @click="delete_confirmation(index)"
            ></i>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- Modal to confirm delete companie -->
    <div class="modal" id="deleteCompanies">
      <div class="modal-dialog">
        <div class="modal-content">
          <!-- Modal Header -->
          <div class="modal-header">
            <h4 class="modal-title"></h4>
            <button type="button" class="close" data-dismiss="modal">
              &times;
            </button>
          </div>

          <!-- Modal body -->
          <div class="modal-body">
            <p>
              Are you sure you want to delete
              <strong>{{ companie_name_to_delete }} </strong> from the table?
            </p>
          </div>
          <!-- Modal footer -->
          <div class="modal-footer">
            <button type="button" class="btn btn-danger" data-dismiss="modal">
              Close
            </button>
            <button
              type="button"
              class="btn btn-primary"
              data-dismiss="modal"
              @click="delete_from_array()"
            >
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- Modal to edit companies -->
    <modal name="modalToEditCompany" :adaptive="true" height="auto">
      <div class="modal-header">
        <h4 class="modal-title">Edit Companies</h4>
        <button type="button" class="close" @click="hide_modal()">
          &times;
        </button>
      </div>
      <div class="modal-body">
        <div class="form-group">
          <label for="usr">Company:</label>
          <input type="text" class="form-control" v-model="edit_company_name" />
        </div>
        <div class="form-group">
          <label for="sel1">Select list:</label>
          <select
            class="form-control"
            id="sel1"
            v-model="edit_company_category"
          >
            <option
              v-for="(item, index) in companie_category"
              :value="item.id"
              :key="'category' + index"
              >{{ item.category }}</option
            >
          </select>
        </div>
        <div class="form-group">
          <label for="usr">City:</label>
          <input type="text" class="form-control" v-model="edit_company_city" />
        </div>
        <p class="text-danger" v-show="edit_message_for_editing_data == true">
          All fields need to be filled in !!!
        </p>
      </div>
      <div class="modal-footer">
        <button
          type="button"
          class="btn btn-primary"
          @click="save_data_in_company_array()"
        >
          Save
        </button>
      </div>
    </modal>
    <!-- Modal to add company -->
    <modal name="modalToAddCompany" :adaptive="true" height="auto">
      <div class="modal-header">
        <h4 class="modal-title">Add Companies</h4>
        <button
          type="button"
          class="close"
          @click="$modal.hide('modalToAddCompany')"
        >
          &times;
        </button>
      </div>
      <div class="modal-body">
        <div class="form-group">
          <label for="usr"> Add Company name:</label>
          <input type="text" class="form-control" v-model="add_company_name" />
        </div>
        <div class="form-group">
          <label for="sel1">Select list:</label>

          <select class="form-control" id="sel1" v-model="add_company_category">
            <option value="" selected disabled>Select one</option>
            <option
              v-for="(item, index) in companie_category"
              :value="item.id"
              :key="'category' + index"
              >{{ item.category }}</option
            >
          </select>
        </div>
        <div class="form-group">
          <label for="usr">City:</label>
          <input type="text" class="form-control" v-model="add_company_city" />
        </div>
        <p class="text-danger" v-show="add_company_message_error == true">
          All fields need to be filled in !!!
        </p>
      </div>
      <div class="modal-footer">
        <button
          type="button"
          class="btn btn-primary"
          @click="add_new_company()"
        >
          Add Company
        </button>
      </div>
    </modal>
  </div>
</template>
<script>
export default {
  name: "Companies",
  props: {
    all_companies: Array,
    companie_category: Array,
  },
  data() {
    return {
      company_array_with_all_data: [],
      index_of_company: "",
      companie_name_to_delete: "",
      edit_company_name: "",
      edit_company_category: "",
      edit_company_city: "",
      edit_message_for_editing_data: false,
      showModal: false,
      add_company_message_error: false,
      add_company_name: "",
      add_company_category: "",
      add_company_city: "",
    };
  },
  mounted() {
    this.show_companies();
  },
  methods: {
    show_companies() {
      this.company_array_with_all_data = [];
      this.all_companies.forEach((item) => {
        let category_index = this.companie_category.findIndex(
          (a) => a.id == item.category_id
        );
        let category_name = this.companie_category[category_index].category;
        this.company_array_with_all_data.push({
          id: item.id,
          name: item.name,
          adress: item.adress,
          category: category_name,
        });
      });
    },
    delete_confirmation(h) {
      this.index_of_company = h;
      let companie_name = this.company_array_with_all_data[h].name;
      this.companie_name_to_delete = companie_name;
    },
    delete_from_array() {
      this.all_companies.splice(this.index_of_company, 1);
      this.show_companies();
    },
    show_data_to_edit(h) {
      this.showModal = true;
      this.index_of_company = h;
      this.edit_company_name = this.company_array_with_all_data[h].name;
      this.edit_company_category = this.company_array_with_all_data[h].id;
      this.edit_company_city = this.company_array_with_all_data[h].adress;
      this.$modal.show("modalToEditCompany");
    },
    save_data_in_company_array() {
      let a = this.index_of_company;
      if (this.edit_company_name == "" || this.edit_company_city == "") {
        this.edit_message_for_editing_data = true;
      } else {
        this.all_companies[a].name = this.edit_company_name;
        this.all_companies[a].category_id = this.edit_company_category;
        this.all_companies[a].adress = this.edit_company_city;
        this.showModal = false;
        this.show_companies();
        this.$modal.hide("modalToEditCompany");
      }
    },
    hide_modal() {
      this.$modal.hide("modalToEditCompany");
    },
    add_new_company() {
      if (
        this.add_company_name == "" ||
        this.add_company_category == "" ||
        this.add_company_city == ""
      ) {
        this.add_company_message_error = true;
      } else {
        let max_id = Math.max.apply(
          Math,
          this.all_companies.map(function(o) {
            return o.id;
          })
        );
        this.all_companies.push({
          id: max_id + 1,
          name: this.add_company_name,
          category_id: this.add_company_category,
          adress: this.add_company_city,
        });
        this.add_company_message_error = false;
        this.show_companies();
        this.$modal.hide("modalToAddCompany");
        this.add_company_name = "";
        this.add_company_category = "";
        this.add_company_city = "";
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
