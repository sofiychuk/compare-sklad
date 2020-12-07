
<template>
  <div class="shop-sklad">
        <p class="count">{{count}}</p>
    <div class="sizeTrigger" >
      <div class="switch-btn" @click="triggerSize"></div>
      <p>Вкл/Викл розмір</p>
    </div>
    <button @click="saveInObj" class="shop-sklad-btn">Зберегти</button>
    <p>
      Статус:
      <span v-if="shopSkladStatus" class="green">Збережено</span>
      <span v-else class="red">Незбережено</span>
    </p>
    <p class="orange">
      <b>Склад магазину</b>
    </p>
    <div class="wrapper">
      <div class="wrapper-textarea">
        <textarea
          name="article"
          id="shop-sklad-article"
          cols="12"
          rows="50"
          v-model="article"
          placeholder="Артикул"
        ></textarea>
        <button class="refresh" @click="refreshArticle">
          <img src="../assets/arrow2.png" class="arrow-refresh" id="first" alt />
        </button>
      </div>
      <div class="wrapper-textarea">
        <textarea
          name="size"
          id="shop-sklad-size"
          cols="12"
          rows="50"
          v-model="size"
          placeholder="Розмір"
          :disabled="sizeDisabled"
        ></textarea>
        <button class="refresh" @click="refreshArticle" :disabled="sizeDisabled">
          <img src="../assets/arrow2.png" class="arrow-refresh" id="second" alt />
        </button>
      </div>
      <div class="wrapper-textarea">
        <textarea
          name="quantity"
          id="shop-sklad-quantity"
          cols="12"
          rows="50"
          v-model="quantity"
          placeholder="Кількість"
        ></textarea>
        <button class="refresh" @click="refreshArticle">
          <img src="../assets/arrow2.png" class="arrow-refresh" id="third" alt />
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // shopSkladObject: {},
      count: 0,
      article: "",
      size: "",
      quantity: "",
      degRefresh1: 0,
      degRefresh2: 0,
      degRefresh3: 0,
      shopSkladStatus: false,
      sizeDisabled: true,
    };
  },
  computed: {
    // console.log(this.str)
  },
  methods: {
    saveInObj() {
      let shopSkladObject = {};
      let key1 = this.article.split("\n");
      let key2 = this.size.split("\n");
      let key3 = this.quantity.split("\n");
      if (!this.sizeDisabled) {
        let result = key1.map((item, i) => {
          return (item += "-" + key2[i]);
        });
        key1 = result
        console.log("gavno", result);
      }
      key1.forEach((item, index) => {
        shopSkladObject[item] = key3[index];
      });
      for (let key in shopSkladObject) {
        if (key.length > 0 && shopSkladObject[key].length > 0) {
          this.shopSkladStatus = true;
        } else  {
          alert(
            "Котусік! Має бути заповнено обов'язково всі колонки магазинного складу!"
          );
          this.shopSkladStatus = false;
          shopSkladObject = {};
        }
      }
      this.$emit("shopSkladObject", shopSkladObject);
       this.count = Object.keys(shopSkladObject).length;
      console.log(shopSkladObject);
    },
    triggerSize(e) {
      e.target.classList.toggle("switch-on");
      this.sizeDisabled = !this.sizeDisabled;
    },
    refreshArticle(e) {
      this.shopSkladStatus = false;
      let choseRefresh = e.target;
      let degRefresh;
      if (choseRefresh.getAttribute("id") == "first") {
        this.article = "";
        this.degRefresh1 += 360;
        degRefresh = this.degRefresh1;
      } else if (choseRefresh.getAttribute("id") == "second") {
        this.size = "";
        this.degRefresh2 += 360;
        degRefresh = this.degRefresh2;
      } else {
        this.quantity = "";
        this.degRefresh3 += 360;
        degRefresh = this.degRefresh3;
      }

      choseRefresh.setAttribute(
        "style",
        "transform: rotate(" + degRefresh + "deg); transition: .8s"
      );
    },
  },
};
</script>

<style scoped>
textarea {
  height: calc(100vh - 175px) !important;
  padding-right: 15px;
}
.shop-sklad {
  position: relative;
}
.count{
  position: absolute;
  right: 5px;
  top: 3px;
  padding: 0;
  margin: 0;
}
.shop-sklad .wrapper-textarea:first-child, .shop-sklad .wrapper-textarea:nth-child(2){
  margin-right: 5px;
}
.switch-btn {
  display: inline-block;
  width: 50px; /* ширина */
  height: 25px; /* высота */
  border-radius: 19px; /* радиус скругления */
  background: #bfbfbf; /* цвет фона */
  z-index: 0;
  margin: 0;
  padding: 0;
  border: none;
  cursor: pointer;
  position: relative;
  transition-duration: 300ms; /* анимация */
}
.switch-btn::after {
  content: "";
  height: 20px;
  width: 20px;
  border-radius: 50%;
  background: #fff; /* цвет кнопки */
  top: 3px; /* положение кнопки по вертикали относительно основы */
  left: 3px; /* положение кнопки по горизонтали относительно основы */
  transition-duration: 300ms; /* анимация */
  position: absolute;
  z-index: 1;
}
.switch-on {
  background: #118c4e;
}
.switch-on::after {
  left: 27px;
}
.sizeTrigger {
  position: absolute;
  margin: 15px 5px;
}
.sizeTrigger p {
  font-size: 10px;
  margin: 0;
  padding: 0;
}

p {
  margin: 10px 0;
  padding: 0;
}
.shop-sklad {
  border: 1px solid orange;
  padding: 0 5px;
}
.orange {
  color: orange;
}
.red {
  color: red;
}
.green {
  color: green;
}
.shop-sklad-btn {
  position: relative;
  display: inline-block;
  color: #777674;
  font-weight: bold;
  text-decoration: none;
  text-shadow: rgba(255, 255, 255, 0.5) 1px 1px,
    rgba(100, 100, 100, 0.3) 3px 7px 3px;
  user-select: none;
  padding: 1em 2em;
  outline: none;
  border-radius: 3px / 100%;
  background-image: linear-gradient(
      45deg,
      rgba(255, 255, 255, 0) 30%,
      rgba(255, 255, 255, 0.8),
      rgba(255, 255, 255, 0) 70%
    ),
    linear-gradient(
      to right,
      rgba(255, 255, 255, 1),
      rgba(255, 255, 255, 0) 20%,
      rgba(255, 255, 255, 0) 90%,
      rgba(255, 255, 255, 0.3)
    ),
    linear-gradient(
      to right,
      rgba(125, 125, 125, 1),
      rgba(255, 255, 255, 0.9) 45%,
      rgba(125, 125, 125, 0.5)
    ),
    linear-gradient(
      to right,
      rgba(125, 125, 125, 1),
      rgba(255, 255, 255, 0.9) 45%,
      rgba(125, 125, 125, 0.5)
    ),
    linear-gradient(
      to right,
      rgba(223, 190, 170, 1),
      rgba(255, 255, 255, 0.9) 45%,
      rgba(223, 190, 170, 0.5)
    ),
    linear-gradient(
      to right,
      rgba(223, 190, 170, 1),
      rgba(255, 255, 255, 0.9) 45%,
      rgba(223, 190, 170, 0.5)
    );
  background-repeat: no-repeat;
  background-size: 200% 100%, auto, 100% 2px, 100% 2px, 100% 1px, 100% 1px;
  background-position: 200% 0, 0 0, 0 0, 0 100%, 0 4px, 0 calc(100% - 4px);
  box-shadow: rgba(0, 0, 0, 0.5) 3px 10px 10px -10px;
}
.shop-sklad-btn:hover {
  transition: 0.5s linear;
  background-position: -200% 0, 0 0, 0 0, 0 100%, 0 4px, 0 calc(100% - 4px);
}
.shop-sklad-btn:active {
  top: 1px;
}
.wrapper {
  display: flex;
}
.wrapper-textarea {
  position: relative;
}
.refresh {
  position: absolute;
  border: none;
  background: transparent;
  z-index: 10;
  width: 25px;
  display: flex;
  top: 0;
  right: 0px;
  padding: 3px;
}
.refresh:active {
  border: none;
  outline: none;
  transition: 1s;
}
button:active,
button:focus {
  outline: none !important;
}
.arrow-refresh {
  width: 100%;
}
</style>