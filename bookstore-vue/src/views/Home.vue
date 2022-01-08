<template>
  <div class="home">
    <div class="sidebar_nody">
      <input class="input" type="checkbox" id="btn6" />
      <label for="btn6" class="icon22">
        <span class="fa fa-bars" id="bars22"
          ><img src="../image_test/menu.png" alt=""
        /></span>
        <span class="fa fa-bars" id="times22"
          ><img src="../image_test/x.png" alt=""
        /></span>
      </label>
      <div class="sidebar">
        <div class="Box">
          <h2>Category items</h2>
          <ul>
            <li
              v-for="category in categories"
              :key="category"
              @click="selecteditem(category)"
            >
              {{ category.charAt(0).toUpperCase() + category.slice(1) }}
            </li>
          </ul>
        </div>
      </div>
      <div class="prod">
        <div class="page">
          <img src="../image_test/left.png" alt="" @click="prev()" />
          <h1>{{ page_num + 1 }}</h1>
          <img src="../image_test/right.png" alt="" @click="next()" />
        </div>
        <div class="pro_con">
          <Product
            class="prodect"
            v-for="product in products"
            :key="product.id"
            :req_product="product"
            @update-cart="updateCart"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Product from "@/components/product.vue";
// import uniq from "lodash/uniq";
/*@update-cart="updateCart"*/
export default {
  name: "Home",
  data() {
    return {
      page_num: 0,
      User: "",
      categories: [],
      products: [],
    };
  },
  mounted() {
    // console.log(window.sessionStorage.getItem("token"));
    // console.log(window.sessionStorage.getItem("userType"));
    this.$store.commit({
      type: "changeType",
      usertype: window.sessionStorage.getItem("userType"),
    });
    // if (
    //   window.sessionStorage.getItem("token") != "" &&
    //   window.sessionStorage.getItem("token") != null
    // ) {
    //   this.$store.state.sign = true;
    // } else {
    //   this.$store.state.sign = false;
    // }

    this.User = this.$route.params.data;

    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    var raw = JSON.stringify(Number(this.page_num) + 1);

    var requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: raw,
      redirect: "follow",
    };

    fetch("http://127.0.0.1:8080/product/product_list", requestOptions)
      .then((response) => response.json())
      .then((result) => {
        // console.log(result);
        this.products = result;
        // this.categories = uniq(this.products.map(({ category }) => category));
      })
      .catch((error) => console.log("error", error));

    var requestOptions1 = {
      method: "POST",
      redirect: "follow",
    };

    fetch("http://127.0.0.1:8080/product/categories", requestOptions1)
      .then((response) => response.json())

      .then((result) => {
        this.categories = result;
        this.categories.push("All");
      })
      .catch((error) => console.log("error", error));
    /*fetch("http://127.0.0.1:8080/product/categories")
      .then((response) => response.json())
      .then((result) => {
        console.log(result);
        this.categories = result;
      })
      .catch((error) => console.log("error", error));*/
  },

  components: {
    Product,
  },
  methods: {
    selecteditem: function (name) {
      console.log(name);
      if (name == "All") {
        this.get_product();
      } else {
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");

        var raw = JSON.stringify({
          page: Number(this.page_num) + 1,
          category: name,
        });

        var requestOptions = {
          method: "POST",
          headers: myHeaders,
          body: raw,
          redirect: "follow",
        };

        fetch(
          "http://127.0.0.1:8080/product/product_list_category",
          requestOptions
        )
          .then((response) => response.json())
          .then((result) => {
            this.products = result;
          })
          .catch((error) => console.log("error", error));
      }
    },
    prev: function () {
      if (this.page_num != 0) {
        this.page_num--;
        this.products = {};
        this.get_product();
      }
    },
    next: function () {
      if (this.products.length == 50) {
        this.page_num++;
        this.products = {};
        this.get_product();
      }
    },
    get_product: function () {
      var myHeaders1 = new Headers();
      myHeaders1.append("Content-Type", "application/json");
      var raw1 = JSON.stringify(Number(this.page_num) + 1);

      var requestOptions2 = {
        method: "POST",
        headers: myHeaders1,
        body: raw1,
        redirect: "follow",
      };

      fetch("http://127.0.0.1:8080/product/product_list", requestOptions2)
        .then((response) => response.json())
        .then((result) => {
          console.log(result);
          if (result.length > 0) {
            this.products = result;
          }
        })
        .catch((error) => console.log("error", error));
    },
    updateCart: function () {
      this.get_product();
    },
  },
};
</script>
<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.sidebar_nody {
  height: 100%;
  width: 100%;
  display: grid;
  grid-template-columns: 300px auto;
  position: relative;
}
.sidebar {
  grid-column: 1 / 2;
  width: 300px;
  min-height: 100vh;
  background: #f6f6f6;
}
.sidebar .Box {
  width: 100%;
}
.sidebar .Box h2 {
  color: #fff;
  background: #6200ee;
  padding: 10px 20px;
  font-size: 30px;
  font-weight: 700;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}
.sidebar .Box ul {
  position: relative;
  background: #fff;
}
.sidebar .Box ul:hover li {
  opacity: 0.4;
}
.sidebar .Box ul li {
  list-style: none;
  font-size: 25px;
  padding: 10px;
  width: 100%;
  background: #fff;
  box-shadow: 0 5px 25px rgba(0, 0, 0, 0.1);
  transition: transform 0.5s;
}
.sidebar .Box ul li:hover {
  cursor: pointer;
  background: rgb(17, 205, 238);
  box-shadow: 0 5px 25px rgba(0, 0, 0, 0.2);
  transform: scaleY(1.1);
  z-index: 100;
  color: rgb(0, 0, 0);
  opacity: 1;
}
.pro_con {
  grid-column: 2;
  display: flex;
  flex-flow: wrap;
  justify-content: center;
}
.prod .page h1 {
  display: inline-flex;
  height: 50px;
  width: 50px;
  margin: 0 10px 0 10px;
  border-radius: 50%;
  background: #3b5efa;
  color: white;
  justify-content: center;
  align-items: center;
}
.prod .page img {
  cursor: pointer;
  border-radius: 50%;
  transition: transform 0.5s;
}
.prod .page img:hover {
  border-color: cyan;
  transform: scale(1.1);
  box-shadow: 0 0 5px #33ffff, 0 0 5px #33ffff;
}
#btn6 {
  display: none;
}
.icon22 {
  position: relative;
  margin: auto;
  display: none;
}
.icon22 span {
  position: absolute;
  color: white;
  font-size: 35px;
}
.icon22 span img {
  border-radius: 50%;
  padding: 10px;
  border-style: solid;
}
.icon22 span img:hover {
  cursor: pointer;
  border-color: cyan;
  box-shadow: 0 0 5px #33ffff, 0 0 5px #33ffff;
}
@media screen and (max-width: 1000px) {
  .sidebar {
    height: 0;
    visibility: hidden;
    background: none;
    transition: 0.5s;
  }
  .sidebar_nody {
    height: 100%;
    width: 100%;
    display: grid;
    grid-template-columns: 0px auto;
  }
  .icon22 {
    display: block;
    position: absolute;
    top: 0px;
    left: 0px;
    cursor: pointer;
  }
  .icon22 #times22 {
    display: none;
  }
  .pro_con {
    opacity: 1;
  }
  #btn6:checked ~ .icon22 #times22 {
    display: block;
  }
  #btn6:checked ~ .icon22 #bars22 {
    display: none;
  }
  #btn6:checked ~ .sidebar {
    margin-top: 50px;
    margin-left: 65px;
    height: 100%;
    visibility: visible;
    z-index: 5;
    background: none;
  }
}
</style>
