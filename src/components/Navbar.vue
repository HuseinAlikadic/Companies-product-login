<template>
  <div class="topnav navbar">
    <a class="active" href="#home">Home</a>
    <a href="#about">About</a>
    <a href="#contact">Contact</a>
    <a href="" v-show="log_in_user != ''">Welcome {{ user_name_login }}</a>
    <div class="login-container">
      <form>
        <input
          class="input-user"
          type="text"
          placeholder="Username"
          name="username"
          autocomplete="on"
          v-model="user_email"
          v-show="hide_login_and_register == false"
        />
        <input
          class="input-user"
          type="password"
          placeholder="Password"
          name="psw"
          autocomplete="off"
          v-model="user_password"
          v-show="hide_login_and_register == false"
        />
        <button
          type="button"
          v-show="hide_login_and_register == true"
          @click="log_out()"
        >
          Log Out
        </button>
        <button
          type="button"
          v-show="hide_login_and_register == false"
          @click="$modal.show('addUserModal')"
        >
          Register
        </button>
        <button
          type="button"
          v-show="hide_login_and_register == false"
          @click="user_log_in()"
        >
          Login
        </button>
      </form>
    </div>
    <!-- Modal to add new user -->
    <modal name="addUserModal" :adaptive="true" height="auto">
      <div class="modal-header">
        <h4 class="modal-title">Add new user:</h4>
        <button
          type="button"
          class="close"
          @click="$modal.hide('addUserModal')"
        >
          &times;
        </button>
      </div>

      <!-- Modal body -->
      <div class="modal-body">
        <div class="form-group">
          <label for="usr">Name:</label>
          <input type="text" class="form-control" v-model="add_user_name" />
        </div>
        <div class="form-group">
          <label for="usr">Email:</label>
          <input type="text" class="form-control" v-model="add_user_email" />
        </div>
        <div class="form-group">
          <label for="usr">Password:</label>
          <input
            type="password"
            class="form-control"
            v-model="add_user_password"
          />
        </div>
        <p class="text-danger" v-show="add_new_user_error_message == true">
          You need to fill in all the fields !!!
        </p>
      </div>

      <!-- Modal footer -->
      <div class="modal-footer">
        <button type="button" class="btn btn-primary" @click="add_new_user()">
          Save
        </button>
      </div>
    </modal>
  </div>
</template>
<script>
export default {
  name: "Navbar",
  props: { registred_user: Array },
  data() {
    return {
      log_in_user: [],
      user_email: "",
      user_password: "",
      hide_login_and_register: false,

      hide_log_out: true,
      add_user_name: "",
      add_user_email: "",
      add_user_password: "",
      add_new_user_error_message: false,
      user_name_login: "",
    };
  },
  methods: {
    user_log_in() {
      let user_found = false;
      let user_index = "Nije";
      // let verifikation_user = false;
      this.registred_user.forEach((item, index) => {
        if (
          this.user_email == item.email &&
          this.user_password == item.password
        ) {
          user_found = true;
          // TODO druga opcija za push u array
          //   this.log_in_user.push({
          //   id: item.id,
          //   email: item.email,
          //   name: item.name,
          //   password: item.password,
          // });
          user_index = index;
        }
      });
      if (user_found == true) {
        this.log_in_user.push({
          id: this.registred_user[user_index].id,
          email: this.registred_user[user_index].email,
          name: this.registred_user[user_index].name,
          password: this.registred_user[user_index].password,
        });
        // this.user_name_login = "";
        this.$parent.show_content = true;
        this.hide_login_and_register = true;
        this.user_name_login = this.log_in_user[0].name;
      } else {
        this.$parent.show_content = false;
      }
      this.user_email = "";
      this.user_password = "";
    },
    add_new_user() {
      let max_id = Math.max.apply(
        Math,
        this.registred_user.map(function(o) {
          return o.id;
        })
      );
      if (
        this.add_user_name == "" ||
        this.add_user_email == "" ||
        this.add_user_password == ""
      ) {
        this.add_new_user_error_message = true;
      } else {
        this.registred_user.push({
          id: max_id + 1,
          name: this.add_user_name,
          email: this.add_user_email,
          password: this.add_user_password,
        });
        this.log_in_user.push({
          id: max_id + 1,
          name: this.add_user_name,
          email: this.add_user_email,
          password: this.add_user_password,
        });
        this.user_name = this.log_in_user.name;
        this.hide_login_and_register = true;
        this.$parent.show_content = true;
        this.$modal.hide("addUserModal");
        this.add_user_name = "";
        this.add_user_email = "";
        this.add_user_password = "";

        this.log_in_user.forEach((item) => {
          this.user_name_login = item.name;
        });
      }
    },
    log_out() {
      this.log_in_user = [];
      this.hide_login_and_register = false;
      this.$parent.show_content = false;
      this.user_name_login = "";
    },
  },
};
</script>
<style scoped>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}

.topnav {
  overflow: hidden;
  background-color: #e9e9e9;
}

.topnav a {
  float: left;
  display: block;
  color: black;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
  font-size: 17px;
}

.topnav a:hover {
  background-color: #ddd;
  color: black;
}

.topnav a.active {
  background-color: #2196f3;
  color: white;
}

.topnav .login-container {
  float: right;
}

.input-user {
  padding: 6px;
  margin-top: 8px;
  font-size: 17px;
  border: none;
  width: 220px;
  margin-right: 5px;
}
.input-user {
  padding: 6px;
  margin-top: 8px;
  font-size: 17px;
  border: none;
  width: 220px;
  margin-right: 5px;
}

.topnav .login-container button {
  float: right;
  padding: 6px 10px;
  margin-top: 8px;
  margin-right: 16px;
  background-color: #555;
  color: white;
  font-size: 17px;
  border: none;
  cursor: pointer;
}

.topnav .login-container button:hover {
  background-color: green;
}

@media screen and (max-width: 600px) {
  .topnav .login-container {
    float: none;
  }
  .topnav a,
  .topnav input[type="text"],
  .topnav .login-container button {
    float: none;
    display: block;
    text-align: left;
    width: 100%;
    margin: 0;
    padding: 14px;
  }
  .topnav input[type="text"] {
    border: 1px solid #ccc;
  }
}
</style>
