<template>
  <div id="app">
    <div class="main-panel">
      <h2>Панель керування</h2>
      <hr />
      <div class="checkbox-result">
        <p>Вибір елементів результату:</p>
        <input id="article" type="checkbox" value="Article" v-model="checkboxResult" />
        <label for="article">Артикуль</label>
        <br />
        <input id="quantity" type="checkbox" value="Quantity" v-model="checkboxResult" />
        <label for="quantity">Кількість</label>
      </div>
      <hr />
      <div class="choice-calculation">
        <p>Вибір вичислення:</p>
        <input id="sum-sklad" name="calculation" type="radio" value="sumSklad" v-model="choiseCalc" />
        <label for="sum-sklad">Сума складів</label>
        <br />
        <input
          id="balance-shop-main"
          name="calculation"
          type="radio"
          value="balanceMain"
          v-model="choiseCalc"
        />
        <label for="balance-shop-main">Залишки для магазину(тільки головний)</label>
        <br />
        <input
          id="balance-shop-other"
          name="calculation"
          type="radio"
          value="balanceMainOther"
          v-model="choiseCalc"
        />
        <label for="balance-shop-other">Залишки для магазину (головний+додатковий)</label>
      </div>
      <hr />
      <button class="result-button" @click="result">
        <p>Порахувати!</p>
      </button>
      <button class="clear-result" @click="clearResult">
        <p>Почистити результат</p>
      </button>
    </div>
    <mainSklad @mainSkladObject="reciveObjectMain"></mainSklad>
    <otherSklad @otherSkladObject="reciveObjectOther"></otherSklad>
    <shopBalance @shopSkladObject="reciveObjectShop"></shopBalance>
    <div class="result-wrapper">
      <h3>Result:</h3>
      <div class="result-text-wrapper">
        <p v-for="text in resultText" :key="text" >{{text}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import mainSklad from "./components/mainSklad.vue";
import otherSklad from "./components/otherSklad.vue";
import shopBalance from "./components/shopBalance.vue";

export default {
  name: "App",
  data() {
    return {
      choiseCalc: "",
      checkboxResult: [],
      mainObj: {},
      mainObj2: {},
      otherObj: {},
      shopObj: {},
      resultText: [],
    };
  },
  components: {
    mainSklad,
    otherSklad,
    shopBalance,
  },
  methods: {
    reciveObjectMain(obj) {
      this.mainObj = obj;
      this.mainObj2 = Object.assign({}, obj);
      console.log(this.mainObj, "mainSkladObject");
      console.log(this.mainObj2, "mainSkladObject2");
    },
    reciveObjectOther(obj) {
      this.otherObj = obj;
      console.log(this.mainObj, "mainSkladObject");
    },
    reciveObjectShop(obj) {
      this.shopObj = obj;
      console.log(this.mainObj, "mainSkladObject");
    },
    result() {
      switch (this.choiseCalc) {
        case "sumSklad":
          this.resultSum();
          break;
        case "balanceMain":
          this.balanceMain();
          break;
        case "balanceMainOther":
          this.balanceMainOther();
          break;
        default:
          alert("Нет таких значений");
      }
      // if (this.choiseCalc == "sumSklad") {
      //   this.resultSum();
      // }
    },
    clearResult() {
      this.mainObj = Object.assign({}, this.mainObj2);
      console.log(this.mainObj, "mainSkladObject");
      console.log(this.mainObj2, "mainSkladObject2");
      this.resultText = [];
    },
    resultSum() {
      for (let key3 in this.otherObj) {
        let count = 0;
        for (let key4 in this.mainObj) {
          if (key3.toLowerCase() == key4.toLowerCase()) {
            this.mainObj[key4] = +this.mainObj[key4] + +this.otherObj[key3];
            count = 1;
          }
        }
        if (count == 0) {
          this.mainObj[key3] = this.otherObj[key3];
        }
      }
      if (this.checkboxResult.length == 2) {
        this.resultBoth(this.mainObj);
        return;
      }
      if (this.checkboxResult[0] == "Article") {
        this.resultWithArticle(this.mainObj);
      }
      if (this.checkboxResult[0] == "Quantity") {
        this.resultWithQuantity(this.mainObj);
      }
    },
    balanceMain() {
      for (let key1 in this.shopObj) {
        let indexOf = true;
        for (let key2 in this.mainObj) {
          if (key1.toLowerCase() == key2.toLowerCase()) {
            this.shopObj[key1] = this.mainObj[key2];
            indexOf = false;
          }
        }
        if (indexOf) {
          this.shopObj[key1] = 0;
        }
      }
      if (this.checkboxResult.length == 2) {
        this.resultBoth(this.shopObj);
        return;
      }
      if (this.checkboxResult[0] == "Article") {
        this.resultWithArticle(this.shopObj);
      }
      if (this.checkboxResult[0] == "Quantity") {
        this.resultWithQuantity(this.shopObj);
      }
    },
    balanceMainOther() {
      for (let key3 in this.otherObj) {
        let count = 0;
        for (let key4 in this.mainObj) {
          if (key3.toLowerCase() == key4.toLowerCase()) {
            this.mainObj[key4] = +this.mainObj[key4] + +this.otherObj[key3];
            count = 1;
          }
        }
        if (count == 0) {
          this.mainObj[key3] = this.otherObj[key3];
        }
      }
      for (let key1 in this.shopObj) {
        let indexOf = true;
        for (let key2 in this.mainObj) {
          if (key1.toLowerCase() == key2.toLowerCase()) {
            this.shopObj[key1] = this.mainObj[key2];
            indexOf = false;
          }
        }
        if (indexOf) {
          this.shopObj[key1] = 0;
        }
      }
      if (this.checkboxResult.length == 2) {
        this.resultBoth(this.shopObj);
        return;
      }
      if (this.checkboxResult[0] == "Article") {
        this.resultWithArticle(this.shopObj);
      }
      if (this.checkboxResult[0] == "Quantity") {
        this.resultWithQuantity(this.shopObj);
      }
    },
    resultWithArticle(obj) {
      for (let key in obj) {
        this.resultText.push(key);
      }
    },
    resultWithQuantity(obj) {
      for (let key in obj) {
        this.resultText.push(obj[key]);
      }
    },
    resultBoth(obj) {
      for (let key in obj) {
        let togetherBecause = key + ": " + obj[key];
        this.resultText.push(togetherBecause);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
  margin-left: 300px;
  display: flex;
  justify-content: space-around;
}
.main-panel {
  position: absolute;
  box-sizing: border-box;
  top: 0;
  left: 0;
  /* border: 1px solid black; */
  background-color: lightblue;
  width: 250px;
  height: 100vh;
}
textarea {
  height: calc(100vh - 175px) !important;
  padding-right: 15px;
}
.result-wrapper {
  border: 1px solid green;
  padding: 0 5px;
}
.checkbox-result {
  padding-left: 10px;
  text-align: left;
  margin: 20px 0;
}
.choice-calculation {
  padding-left: 10px;
  text-align: left;
  margin: 20px 0;
}
label {
  font-size: 14px;
}
.result-button {
  box-shadow: 0px 10px 14px -7px #3e7327;
  background: linear-gradient(to bottom, #77b55a 5%, #72b352 100%);
  background-color: #77b55a;
  border-radius: 4px;
  border: 1px solid #4b8f29;
  display: inline-block;
  cursor: pointer;
  color: #ffffff;
  font-family: Arial;
  font-size: 13px;
  font-weight: bold;
  padding: 1px 10px;
  text-decoration: none;
  text-shadow: 0px 1px 0px #5b8a3c;
  margin: 20px 0;
}
.result-button:hover {
  background: linear-gradient(to bottom, #72b352 5%, #77b55a 100%);
  background-color: #72b352;
}
.result-button:active {
  position: relative;
  top: 1px;
}

.clear-result {
  box-shadow: inset 0px 39px 0px -24px #e67a73;
  background-color: #e4685d;
  border-radius: 4px;
  border: 1px solid #ffffff;
  display: inline-block;
  cursor: pointer;
  color: #ffffff;
  font-family: Arial;
  font-size: 11px;
  padding: 6px 8px;
  text-decoration: none;
  text-shadow: 0px 1px 0px #b23e35;

  margin: 10px 0 0 10px;
}
.clear-result:hover {
  background-color: #eb675e;
}
.clear-result:active {
  position: relative;
  top: 1px;
}
.result-wrapper{
    height: calc(100vh - 50px);
    width: 130px;
    overflow: auto;
}
.result-text-wrapper{
  width: 100%;
  text-align: left;
}
.result-text-wrapper p{
  font-size: 12px;
  margin: 0;
  padding: 0;
}

</style>
