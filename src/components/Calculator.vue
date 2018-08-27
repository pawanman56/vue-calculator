<template>
  <div class="calculator">
    <div class="display">
      {{ current || '0' }}
    </div>
    <div @click="clear" class="btn">C</div>
    <div @click="sign" class="btn">+/-</div>
    <div @click="percent" class="btn">%</div>
    <div @click="divide" class="btn operators">÷</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="multiply" class="btn operators">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="subtract" class="btn operators">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="add" class="btn operators">+</div>
    <div @click="append('0')" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equals" class="btn operators equals">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: '',
      operator: null,
    }
  },
  methods: {
    clear() {
      this.current = '';
    },
    sign() {
      this.current = this.current.charAt(0) === '-' ?
        this.current.slice(1): `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    multiply() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    subtract() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    equals() {
      this.current = `${this.operator(
        parseFloat(this.current),
        parseFloat(this.previous)
      )}`;
      this.previous = null;
    }
  }
}
</script>

<style scoped>
.calculator {
  display: grid;
  margin: 0 auto;
  width: 310px;
  font-size: 25px;
  text-align: right;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  box-shadow: 5px 5px 10px;
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: #fff;
  padding: 10px 10px 7px;
  font-size: 37px;
  line-height: 1;
  overflow-wrap: break-word;
  border-top-left-radius: 7px;
  border-top-right-radius: 7px;
}

.btn {
  background-color: #eee;
  border: 1px solid #777;
  text-align: center;
  padding: 10px;
}

.btn:hover {
  background-color: #ccc;
  transition: 0.25s linear;
}

.operators {
  background-color: #ffa500;
  color: #fff;
}

.operators:hover {
  background-color: #db8e00;
}

.zero {
  grid-column: 1 / 3;
  border-bottom-left-radius: 7px;
}

.equals {
  border-bottom-right-radius: 7px;
}
</style>
