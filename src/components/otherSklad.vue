
<template>
  <div class="other-sklad">
    <button @click="saveInObj" class="other-sklad-btn">Зберегти</button>
    <p>
      Статус:
      <span v-if="otherSkladStatus" class="green">Збережено</span>
      <span v-else class="red">Незбережено</span>
    </p>
    <p class="violet">
      <b>Додатковий склад</b>
    </p>
    <div class="wrapper">
      <div class="wrapper-textarea">
        <textarea name="article" id="other-sklad-article" cols="12" rows="50" v-model="article" placeholder="Артикуль"></textarea>
        <button class="refresh" @click="refreshArticle">
          <img src="../assets/arrow2.png" class="arrow-refresh" id="first" alt />
        </button>
      </div>
      <div class="wrapper-textarea">
        <textarea name="quantity" id="other-sklad-quantity" cols="12" rows="50" v-model="quantity" placeholder="Кількість"></textarea>
        <button class="refresh" @click="refreshArticle">
          <img src="../assets/arrow2.png" class="arrow-refresh" id="second" alt />
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // otherSkladObject: {},
      article: "",
      quantity: "",
      degRefresh1: 0,
      degRefresh2: 0,
      otherSkladStatus: false,
    };
  },
  computed: {
    // console.log(this.str)
  },
  methods: {
    saveInObj() {
      let otherSkladObject = {};
      let key1 = this.article.split("\n");
      let key2 = this.quantity.split("\n");

      key1.forEach((item, index) => {
        otherSkladObject[item] = key2[index];
      
      });
      for (let key in otherSkladObject) {
        if (key.length > 0 && otherSkladObject[key].length > 0) {
          this.otherSkladStatus = true;
        } else {
          alert("Котусік! Має бути заповнено обов'язково дві колонки додаткового складу!");
          this.otherSkladStatus = false;
          otherSkladObject = {};
        }
      }
      this.$emit('otherSkladObject', otherSkladObject)
        console.log(otherSkladObject);
    },
    refreshArticle(e) {
      this.otherSkladStatus = false;
      let choseRefresh = e.target;
      let degRefresh;
      if (choseRefresh.getAttribute("id") == "first") {
        this.article = "";
        this.degRefresh1 += 360;
        degRefresh = this.degRefresh1;
      } else {
        this.quantity = "";
        this.degRefresh2 += 360;
        degRefresh = this.degRefresh2;
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
.other-sklad .wrapper-textarea:first-child{
  margin-right: 5px;
}
p {
  margin: 10px 0;
  padding: 0;
}
.other-sklad{
  border: 1px solid violet;
  padding: 0 5px;
}
.violet{
  color: violet;
}
.red {
  color: red;
}
.green {
  color: green;
}
.other-sklad-btn {
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
.other-sklad-btn:hover {
  transition: 0.5s linear;
  background-position: -200% 0, 0 0, 0 0, 0 100%, 0 4px, 0 calc(100% - 4px);
}
.other-sklad-btn:active {
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