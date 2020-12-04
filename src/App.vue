<template>
  <div id="app">
    <div class="calculator">
      <div class="display">
        <p>{{ display }}</p>
      </div>

      <div class="numpad">
        <div id="7" class="button noselect" v-on:click="onClick('7')">
          <p>7</p>
        </div>
        <div class="button noselect" v-on:click="onClick('8')">
          <p>8</p>
        </div>
        <div class="button noselect" v-on:click="onClick('9')">
          <p>9</p>
        </div>
        <div class="button noselect" v-on:click="onClick('/')">
          <p>/</p>
        </div>
        <div class="button noselect" v-on:click="onClick('4')">
          <p>4</p>
        </div>
        <div class="button noselect" v-on:click="onClick('5')">
          <p>5</p>
        </div>
        <div class="button noselect" v-on:click="onClick('6')">
          <p>6</p>
        </div>
        <div class="button noselect" v-on:click="onClick('*')">
          <p>*</p>
        </div>
        <div class="button noselect" v-on:click="onClick('1')">
          <p>1</p>
        </div>
        <div class="button noselect" v-on:click="onClick('2')">
          <p>2</p>
        </div>
        <div class="button noselect" v-on:click="onClick('3')">
          <p>3</p>
        </div>
        <div class="button noselect" v-on:click="onClick('-')">
          <p>-</p>
        </div>
        <div class="button noselect" v-on:click="onClick('C')">
          <p>C</p>
        </div>
        <div class="button noselect" v-on:click="onClick('0')">
          <p>0</p>
        </div>
        <div class="button noselect" v-on:click="onClick('=')">
          <p>=</p>
        </div>
        <div class="button noselect" v-on:click="onClick('+')">
          <p>+</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import numeral from "numeral";

export default {
  name: "App",
  data() {
    return {
      display: "",
      lock: false
    };
  },
  methods: {
    onClick(key) {
      const number = numeral(key).value();
      const lastChar = this.display.substring(this.display.length - 1);

      if (this.lock) {
        this.display = '';
        this.lock = false;
        return;
      }

      if (typeof number === "number") {
        this.display = this.display + number;
      } else {
        if (
          this.isNotSymbol(lastChar) &&
          this.isSymbol(key) &&
          lastChar !== key &&
          lastChar !== '=' &&
          lastChar !== ''
        ) {
          this.display = this.display + key;

          if (key === '=') {
            this.lock = true;
            const quation = this.getEquation(this.display)
            this.getResult(quation)
          }
          if (key === 'C') {
            this.display = '';
            this.lock = false;
          }
        }
      }
    },
    getEquation(source) {
      let quation = source;
      quation = quation.split(/(\+|\-|\*|\/|\=)/); //eslint-disable-line
      return quation
    },
    getResult(quation) {
      let number = numeral();
      let symbol = null;
      for (let index = 0; index < quation.length; index++) {
        const curr = quation[index];
        if (index === 0) number.set(curr);

        if (typeof (numeral(curr).value()) === 'number') {
          this.calculate(number, symbol, curr)
        } else {
          symbol = curr;
        }
      }
      this.display = this.display + number.value();
    },
    calculate(number, key, next) {
      switch (key) {
        case "+":
          number.add(next);
          break;
        case "-":
          number.subtract(next);
          break;
        case "*":
          number.multiply(next);
          break;
        case "/":
          number.divide(next);
          break;
        case "=":
          number;
          break;
        case "C":
          break;
        default:
          break;
      }
      return number
    },
    isSymbol(key) {
      switch (key) {
        case "+":
          return true;
        case "-":
          return true;
        case "*":
          return true;
        case "/":
          return true;
        case "=":
          return true;
        case "C":
          return true;
        default:
          return false;
      }
    },
    isNotSymbol(key) {
      return !this.isSymbol(key);
    },
  },
};
</script>

<style>
html,
body,
#app {
  padding: 0px;
  margin: 0px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}

#app {
  height: 100vh;
  justify-content: center;
  position: fixed;
  top: 50%;
  left: 50%;
  margin-top: -200px;
  margin-left: -150px;
}

.calculator {
  width: 300px;
  height: 400px;
  background: grey;
  margin: auto;
}
.display {
  padding-left: 20px;
  padding-right: 20px;
  height: 80px;
  border: 1px solid white;
  box-sizing: border-box;
  background-color: rgba(61, 61, 61, 0.95);
}
.display > p {
  font-size: 24px;
  font-weight: bold;
  line-height: auto;
  color: rgba(250, 250, 250, 0.95);
}
.numpad {
  display: grid;
  height: 320px;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr;
}
.button {
  border: 1px solid white;
  box-sizing: border-box;
  height: 80px;
  background-color: rgba(211, 211, 211, 0.8);
}
.button:active {
  background-color: rgba(250, 250, 250, 0.95);
}
.button > p {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
  line-height: auto;
}
</style>
