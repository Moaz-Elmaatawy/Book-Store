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
          <Book
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
import Book from "@/components/book.vue";
// import uniq from "lodash/uniq";
/*@update-cart="updateCart"*/
export default {
  name: "Home",
  data() {
    return {
      page_num: 0,
      User: "",
      categories: ["categories 1","categories 2","categories 3","categories 4"],
      products: [
        {id:1,name:"test",price:3,images:["data:image/jpeg;base64,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"]},
        {id:2,name:"test2",price:3,images:["data:image/jpeg;base64,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"]}
      ],
    };
  },
  mounted() {
    // this.$store.commit({
    //   type: "changeType",
    //   usertype: window.sessionStorage.getItem("userType"),
    // });

    // this.User = this.$route.params.data;

    // var myHeaders = new Headers();
    // myHeaders.append("Content-Type", "application/json");
    // var raw = JSON.stringify(Number(this.page_num) + 1);

    // var requestOptions = {
    //   method: "POST",
    //   headers: myHeaders,
    //   body: raw,
    //   redirect: "follow",
    // };

    // fetch("http://127.0.0.1:8080/product/product_list", requestOptions)
    //   .then((response) => response.json())
    //   .then((result) => {
    //     // console.log(result);
    //     this.products = result;
    //     // this.categories = uniq(this.products.map(({ category }) => category));
    //   })
    //   .catch((error) => console.log("error", error));

    // var requestOptions1 = {
    //   method: "POST",
    //   redirect: "follow",
    // };

    // fetch("http://127.0.0.1:8080/product/categories", requestOptions1)
    //   .then((response) => response.json())

    //   .then((result) => {
    //     this.categories = result;
    //     this.categories.push("All");
    //   })
    //   .catch((error) => console.log("error", error));
  },

  components: {
    Book,
  },
  methods: {
    selecteditem: function (name) {
      console.log(name);
      // if (name == "All") {
      //   this.get_product();
      // } else {
      //   var myHeaders = new Headers();
      //   myHeaders.append("Content-Type", "application/json");

      //   var raw = JSON.stringify({
      //     page: Number(this.page_num) + 1,
      //     category: name,
      //   });

      //   var requestOptions = {
      //     method: "POST",
      //     headers: myHeaders,
      //     body: raw,
      //     redirect: "follow",
      //   };

      //   fetch(
      //     "http://127.0.0.1:8080/product/product_list_category",
      //     requestOptions
      //   )
      //     .then((response) => response.json())
      //     .then((result) => {
      //       this.products = result;
      //     })
      //     .catch((error) => console.log("error", error));
      // }
    },
    prev: function () {
      if (this.page_num != 0) {
        this.page_num--;
        this.products = {};
        // this.get_product();
      }
    },
    next: function () {
      if (this.products.length == 50) {
        this.page_num++;
        this.products = {};
        // this.get_product();
      }
    },
    get_product: function () {
      // var myHeaders1 = new Headers();
      // myHeaders1.append("Content-Type", "application/json");
      // var raw1 = JSON.stringify(Number(this.page_num) + 1);

      // var requestOptions2 = {
      //   method: "POST",
      //   headers: myHeaders1,
      //   body: raw1,
      //   redirect: "follow",
      // };

      // fetch("http://127.0.0.1:8080/product/product_list", requestOptions2)
      //   .then((response) => response.json())
      //   .then((result) => {
      //     console.log(result);
      //     if (result.length > 0) {
      //       this.products = result;
      //     }
      //   })
      //   .catch((error) => console.log("error", error));
    },
    updateCart: function () {
      // this.get_product();
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
