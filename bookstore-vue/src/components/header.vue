<template>
  <input class="input" type="checkbox" id="btn" />
  <nav>
    <div class="logo">
      <strong style="color: rgb(17, 205, 238)"> S</strong>tore
      <strong style="color: rgb(17, 205, 238)">S</strong>hop
    </div>
    <div class="search_box">
      <input type="text" name="" placeholder="Type..." id="serch_id" />
      <img src="../image_test/search.png" alt="" @click="search_op()" />
      <!-- <input type="submit" name="" value="Search" /> -->
    </div>
    <ul>
      <li @click="homePage()">
        <img src="../image_test/home-64.png" alt="" />
      </li>
      <li
        v-if="!this.$store.state.show && this.$store.state.sign"
        @click="cartsuit()"
      >
        <img src="../image_test/shopping1.png" alt="" />
      </li>
      <li v-if="this.$store.state.show" @click="addprod()">
        <img src="../image_test/add.png" alt="" />
      </li>
      <li @click="userPage()"><img src="../image_test/user.png" alt="" /></li>
      <li @click="settingPage()" class="seet">
        <img src="../image_test/setting.png" alt="" />
      </li>
      <li v-if="this.$store.state.sign" @click="signout()">
        <img src="../image_test/sign.png" alt="" />
      </li>
    </ul>

    <label for="btn" class="icon">
      <span class="fa fa-bars" id="bars"
        ><img src="../image_test/menu.png" alt=""
      /></span>
      <span class="fa fa-bars" id="times"
        ><img src="../image_test/x.png" alt=""
      /></span>
    </label>
  </nav>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {};
  },
  methods: {
    search_op: function () {
      let reach_val = document.getElementById("serch_id").value;
      console.log(reach_val);
    },
    userPage: function () {
      if (this.$store.state.sign) {
        this.$router.push("modifyUser");
      } else {
        this.$router.push("Accounts");
      }
    },
    homePage: function () {
      this.$router.push("/");
    },
    cartsuit: function () {
      this.$router.push("cart");
    },
    settingPage: function () {
      this.$router.push("settings");
    },
    signout: function () {
      if (confirm("Do you really want to signout?")) {
        this.$store.commit({
          type: "changeType",
          usertype: null,
        });
        window.sessionStorage.setItem("token", null);
        window.sessionStorage.setItem("userType", null);
        this.$store.state.sign = false;
        this.$router.push("Accounts");
      }
    },
    addprod: function () {
      this.$router.push("productEdit");
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  box-sizing: border-box;
  color: white;
}
b {
  font-size: 40px;
}
nav {
  text-align: initial;
  background: #6200ee;
  display: flex;
  width: 100%;
  justify-content: space-between;
  border-radius: 0 0 30px 30px;
  box-shadow: 5px 5px 30px rgb(141, 148, 148);
}
nav .logo {
  margin-right: 10px;
  font-size: 27px;
  font-weight: 600;
  letter-spacing: 5px;
  line-height: 70px;
  padding-left: 20px;
  display: flex;
  align-items: center;
}
nav .search_box {
  display: none; /**/
  margin: auto 0;
  height: 60px;
  width: 1100px;
  min-width: 20%;
  max-width: 100%;
  position: relative;
}
nav .search_box input {
  position: relative;
  display: inline-block;
  font-size: 20px;
  box-sizing: border-box;
  transition: 0.5s;
}
nav .search_box input[type="text"] {
  text-align: center;
  color: black;
  height: 100%;
  width: 100%;
  border: none;
  outline: none;
  padding: 0 10px;
  border-radius: 25px 25px 25px 25px;
}
nav .search_box img {
  position: absolute;
  left: 1%;
  top: 50%;
  border-radius: 50%;
  transform: translateY(-50%);
}
nav .search_box img:hover {
  cursor: pointer;
  border-color: cyan;
  box-shadow: 10px 10px 25px #33ffff;
}

nav ul {
  display: flex;
  list-style: none;
  align-items: center;
}

nav ul li {
  background: #6200ee;
  margin: 10px 10px;
  border-radius: 50%;
  border-style: solid;
  border-color: #6200ee;
}
nav ul li img {
  cursor: pointer;
  padding: 5px 10px;
  border-radius: 50%;
}
nav ul li:hover {
  cursor: pointer;
  border-color: cyan;
  box-shadow: 0 0 5px #33ffff, 0 0 5px #33ffff;
}
nav .icon {
  position: relative;
  margin: auto;
  display: none;
}
nav .icon span {
  position: absolute;
  color: white;
  font-size: 35px;
}
nav .icon span img {
  border-radius: 50%;
  padding: 10px;
  border-style: solid;
  border-color: #6200ee;
}
nav .icon span img:hover {
  cursor: pointer;
  border-color: cyan;
  box-shadow: 0 0 5px #33ffff, 0 0 5px #33ffff;
}
#btn {
  display: none;
}
nav .input,
nav .show {
  display: none;
}
.seet {
  display: none;
}

@media screen and (max-width: 1000px) {
  nav {
    display: block;
    padding: 0;
    text-align: center;
  }
  nav::after {
    content: "";
    clear: both;
    display: table;
  }
  nav .icon {
    display: inline-block;
    padding: 15px 30px;
  }
  nav .icon {
    display: block;
    position: absolute;
    top: -15px;
    right: 80px;
    cursor: pointer;
  }
  nav .icon #times {
    display: none;
  }
  #btn:checked ~ nav .icon #times {
    display: block;
  }
  #btn:checked ~ nav .icon #bars {
    display: none;
  }
  #btn:checked ~ nav ul {
    max-height: 900px;
    visibility: visible;
  }
  #btn:checked ~ nav .search_box {
    display: inline-flex;
    display: none; /**/
  }
  nav .search_box {
    margin: auto;
    width: 70%;
    display: inline-flex;
    justify-content: center;
    margin-bottom: 15px;
    display: none;
  }
  nav .search_box input[type="text"] {
    text-align: center;
    font-size: 25px;
    width: 90%;
  }
  nav .search_box img {
    left: 7%;
  }
  nav ul {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    margin-right: 0px;
    max-height: 0;
    visibility: hidden;
  }
  nav ul li {
    text-align: center;
  }
  nav ul li a {
    color: white;
    font-size: 28px;
    padding: 25px;
    display: block;
  }
}
</style>
