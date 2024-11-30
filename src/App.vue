<script>
export default {
  data() {
    return {
      darkMode: false,
      output: '0',
      prev: null,
      cur: null,
      operator: null,
      operatorActions: {
        '+': (a, b) => a + b,
        '-': (a, b) => a - b,
        '*': (a, b) => a * b,
        '/': (a, b) => a / b,
      },
    };
  },
  // 모드별 텍스트 출력
  computed: {
    modeText(){
      return this.darkMode ? "Dark Mode Calculator" : "Light Mode Calculator";
    }
  },
  // methods
  methods: {
    clear() {
      this.output = '0';
      this.prev = null;
      this.cur = null;
      this.operator = null;
    },
    calculate(num) {
      if (!this.cur && !this.prev) {
        alert('숫자를 먼저 입력하세요');
        return;
      }
      if (this.operator !== '' && !this.cur) {
        alert('사칙연산 기호를 연달아 누를 수 없습니다');
        return;
      }
      if (num === '=' && this.prev === this.cur) {
        return;
      }
      this.cur = Number(this.cur);
      if (this.operator !== null) {
        this.prev = this.operatorActions[this.operator](this.prev, this.cur);
        if (num === '=') {
          this.output = this.prev;
          this.operator = null;
          this.cur = this.prev;
        } else {
          this.output = null;
          this.operator = num;
          this.cur = null;
        }
      } else {
        this.output = null;
        this.operator = num;
        this.prev = this.cur;
        this.cur = null;
      }
    },
    userInput(num) {
      this.cur = this.cur === null ? num : (this.cur += num);
      this.output = this.cur;
    },
    operation(e) {
      const num = e.currentTarget.value;
      if (num === 'C') {
        this.clear();
      } else if (['+', '-', '*', '/', '='].includes(num)) {
        this.calculate(num);
      } else {
        this.userInput(num);
      }
    },
    dark() {
      document.querySelector('body').classList.add('dark-mode'); //body에 dark-mode 클래스 추가해 스타일변경
      this.darkMode = true;
    },
    light() {
      document.querySelector('body').classList.remove('dark-mode'); //body에 dark-mode 클래스 삭제해 스타일 복원
      this.darkMode = false;
    },
    modeToggle() {
      if (this.darkMode) {
        this.light();
      } else {
        this.dark();
      }
    },
  },
};
</script>

<template>
  <div class="darkTitle">
    <h1>{{ modeText }}</h1>
    <div class="mode-toggle" @click="modeToggle">
      <img src="/moon.png" width="50%">
      <img src="/sun.png" width="40%">
      <div class="toggle">
        <div id="dark-mode" type="checkbox">
        </div>
      </div>
    </div>
  </div>
  <div class="darkTitle">
    
  </div>
  <div class="calculator">
    <form name="forms">
      <input v-model="output" type="text" name="output" readonly />
      <input type="button" class="clear" value="C" @click="operation" />
      <input type="button" class="operator" value="/" @click="operation" />
      <input type="button" value="1" @click="operation" />
      <input type="button" value="2" @click="operation" />
      <input type="button" value="3" @click="operation" />
      <input type="button" class="operator" value="*" @click="operation" />
      <input type="button" value="4" @click="operation" />
      <input type="button" value="5" @click="operation" />
      <input type="button" value="6" @click="operation" />
      <input type="button" class="operator" value="+" @click="operation" />
      <input type="button" value="7" @click="operation" />
      <input type="button" value="8" @click="operation" />
      <input type="button" value="9" @click="operation" />
      <input type="button" class="operator" value="-" @click="operation" />
      <input type="button" class="dot" value="." @click="operation" />
      <input type="button" value="0" @click="operation" />
      <input type="button" class="operator result" value="=" @click="operation" />
    </form>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

body.dark-mode {
  background-color: #222;
}

body.dark-mode .darkTitle h1 {
  color: #fff;
}

.mode-toggle {
  position: relative;
  margin: 5px ;
  width: 44px;
  height: 24px;
  background-color: #a5abba;
  border-radius: 24px;
  cursor: pointer;
  transition: background-color 0.5s ease;
}

.mode-toggle .toggle {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  margin: auto;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background-color: #fff;
  transition: transform 0.5s ease;
}

body.dark-mode .mode-toggle {
  background-color: #555;
}

body.dark-mode .mode-toggle .toggle {
  transform: translateX(20px);
}

.calculator {
  width: 287px;
  border: 1px solid #333;
  background-color: #ccc;
  padding: 5px;
  border-radius: 15px;
}

body.dark-mode .calculator {
  background-color: #333;
}

.calculator form {
  display: grid;
  grid-template-columns: repeat(4, 65px);
  grid-auto-rows: 65px;
  grid-gap: 5px;
}

.calculator form input {
  border: 2px solid #333;
  cursor: pointer;
  font-size: 19px;
  border-radius: 15px;
}

.calculator form input:hover {
  box-shadow: 1px 1px 2px #333;
}

.calculator form .clear {
  background-color: #ed4848;
}

.calculator form .operator {
  background-color: orange;
}

.calculator form .dot {
  background-color: green;
}

.calculator form input[type='text'] {
  grid-column: span 4;
  text-align: right;
  padding: 0 10px;
}

.calculator form .clear {
  grid-column: span 3;
}

.calculator form .result {
  grid-column: span 2;
}

body.dark-mode .calculator form .clear,
body.dark-mode .calculator form .operator,
body.dark-mode .calculator form .dot {
  background-color: #555;
  color: #fff;
}

body.dark-mode .calculator form input {
  color: #555;
}

</style>
