<template>           
  <div id="app">
    <!-- Выбираем тип калькулятора: первый или второй вариант -->
    <div class="padding10">
      <input 
        type="radio" 
        id="one" 
        value="первый" 
        v-model="picked">
      <label 
        class="label" 
        for="one">
        Калькулятор первый вариант
      </label>
      <br>
      <input 
        type="radio" 
        id="two" 
        value="второй" 
        v-model="picked">
      <label 
        class="label" 
        for="two">
        Калькулятор второй вариант
      </label>
      <br>
      <span 
        v-show="picked" 
        class="label">
        Вы выбрали калькулятор {{ picked }} вариант
      </span>
    </div>
    <!-- Калькулятор первый вариант -->
    <div v-show="picked=='первый'">
      <h2>Калькулятор</h2>
      <p>может выполнять несколько действий, например: 2 * 3 + 4 * 5 - 25,</p>
      <p>удалять последний введённый символ и очищать всё.</p>
      <p>Внимание! Корректно работает только с экранной клавиатурой,</p>
      <p>при вводе с клавиатуры необходимо перед и после знака аоифметической операции обязательно добавлять пробел.</p>

      <div 
        v-show="!total" 
        class="result">
      </div>
      <div 
        v-show="total" 
        class="result">{{input}} {{ operation }} {{ operand2 }} = {{ total }}
      </div>
      <div>
        <form>
          <div class="section">
            <input 
              class="btn btn4" 
              aria-label="input" 
              type="text" 
              v-model="input" 
              />
          </div>
        </form>
        <div>
          <div class="section">
            <div 
              v-for="number in num" 
              :key="number">
              <button 
                class="btn" 
                @click="inputNum(number)"
                >
                {{ number }}
              </button>
            </div>
          </div>

          <div class="section">
            <div 
              v-for="operation in operations" 
              :key="operation">
              <button 
                class="btn btn2" 
                @click="inputOp(operation)"
                >{{ operation }}
              </button>
            </div>
          </div>
        </div>  
        <div class="section">     
          <button class="btn btn3" @click="allClear">Очистить всё</button>                       
          <button class="btn btn3" @click="oneClear">Удалить последний символ</button>       
          <button class="btn btn3" @click="calculation">Результат = <strong class="red">{{ total }}</strong></button>
        </div>  
      </div>
    </div>
    <!-- Калькулятор второй вариант -->
    <div v-show="picked=='второй'">
      <h2>Калькулятор</h2>
      <p>домашнее задание 2</p>
      <div 
        class="result">
        {{op1}} {{ operation2 }} {{ op2 }} = {{ total2 }}
      </div>
    <!-- <div v-show="!total" class="result"></div> -->

      <!-- Выбираем какое число вводить: первое или второе? -->
      <div class="padding10">
        <input 
          type="radio" 
          id="three" 
          value="первое" 
          v-model="picked2">
        <label 
          class="label" 
          for="three"
          >
          Первое число
        </label>
        <br>
        <input 
          type="radio" 
          id="four" 
          value="второе" 
          v-model="picked2">
        <label 
          class="label" 
          for="four"
          >
          Второе число
        </label>
        <br>
        <span 
          class="label"
          >
          Введите {{ picked2 }} число
        </span>
      </div>
      <div>
      <!-- Открываем поля ввода для числа: первое или второе? -->
        <form>
          <div class="section">
            <!-- v-show="picked2=='первое'" --> 
            <input 

              class="btn btn4" 
              aria-label="input" 
              type="text" 
              placeholder="Введите первое число" 
              v-model.number="op1"
            />
            <!-- v-show="picked2=='второе'" --> 
            <input
              
              class="btn btn4" 
              aria-label="total" 
              type="text" 
              placeholder="Введите второе число" 
              v-model.number="op2"
            />
          </div>
        </form>
        <div v-show="error" class="error">
          Ошибка: {{ error }}
        </div>
        <!-- Показать / убрать экранную клавиатуру -->
        <div class="padding-left-10">
          <input 
            type="checkbox" 
            id="checkbox" 
            v-model="checked"
          >
          <label 
            class="label" 
            v-show="!checked" 
            for="checkbox"
            >Показать экранную клавиатуру
          </label>
          <label 
            class="label" 
            v-show="checked" 
            for="checkbox"
            >Скрыть экранную клавиатуру
          </label><br><br>
        </div>
      </div>
      <div>
        <!-- кнопки цифр экранной клавиатуры -->
        <div 
          v-show="checked" 
          class="section">
          <div 
            v-show="picked2=='первое'" 
            v-for="number in num" 
            :key="number">
            <button 
              class="btn" 
              @click="inputNum1(number)"
              >{{ number }}
            </button>
          </div> 
          <div 
            v-show="picked2=='второе'" 
            v-for="number in num" 
            :key="number">
            <button 
              class="btn" 
              @click="inputNum2(number)"
              >{{ number }}
            </button>
          </div> 
        </div>
          
        <!-- кнопки арифметических операций экранной клавиатуры -->          
        <div class="section">
          <div 
            v-show="picked2" 
            v-for="operation2 in operations2" 
            :key="operation2">
            <button 
              class="btn btn2" 
              @click="inputOp2(operation2)"
              >{{ operation2 }}
            </button>
          </div>
        </div>
      </div>  
      <div class="section">     
        <button 
          class="btn btn3" 
          @click="allClear"
          >Очистить всё
        </button>    
<!--         <button 
          v-show="picked2=='первое'" 
          class="btn btn3" 
          @click="oneClear1"
          >Удалить первое число
        </button>                       
        <button 
          v-show="picked2=='второе'" 
          class="btn btn3" 
          @click="oneClear2"
          >Удалить второе число
        </button>  -->
        <button 
          v-show="picked2=='первое'"
          class="btn btn3" 
          @click="oneClear3"
          >Удалить последний символ
        </button>      
        <button 
          v-show="picked2=='второе'" 
          class="btn btn3" 
          @click="oneClear4"
          >Удалить последний символ
        </button>          
        <button 
          class="btn btn3" 
          @click="calculation2(operation2)"
          >Результат = <strong class="red">{{ total2 }}</strong>
        </button>
      </div>  
    </div>
  </div>
</template>

<script>
export default {
  name: 'calculator',
  data(){
    return{
      picked: "",
      picked2: "первое",
      checked: "",
      number: "",
      input: "",
      input1: "",
      input2: "",
      operand: "",      
      operand1: "",
      operand2: "",
      op1: "",
      op2: "",
      error: "",
      operation: "",
      operation2: "",
      total: "",
      total2: "",
      num: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
      operations: ["+", "-", "*", "/"],
      operations2: ["+", "-", "*", "/", "возведение в степень", "целочисленное деление"],
    }
  },
  methods: {
    /* ввод чисел первый вариант */
    inputNum(number){
      if(this.input === "" && number === '0')return;
      this.input += number;
    },
    /* ввод знака арифметической операции первый вариант */
    inputOp(operation){ 
     if(this.input === "" && operation != "-")return;
      else if(this.input[this.input.length - 1] === " ")
        this.input = this.input.substring(0, this.input.length - 2) + operation + " ";
      else this.input += " " + operation + " "; 
      // при вводе с клавиатуры надо вручную добавлять пробелы до и после знака арифметической операции
    },
    /* произвести вычисление первый вариант */
    calculation(){
      if(this.input === "")return;
      this.total = this.expressionParser(this.input); 
/*       this.input = ""; */
    },
    /* это первый вариант - ниже в expressionParser пока не всё понятно на 100% */
    expressionParser(expression){
      let numbers = [];
      let ops = [];
      expression = expression.replace(" ", "");
      if(isNaN(expression[expression.length - 2]) && expression.length != 1){
        expression += "0";
      }
      for(let i = 0; i < expression.length; i++){
        if(isNaN(expression[i])){
          let currOp = expression[i];
          while(ops.length > 0 && this.getPriority(currOp) <= this.getPriority(ops[ops.length - 1])){
            this.process(numbers, ops[ops.length - 1]);
            ops.pop();
          }
          ops.push(currOp);
        }
        else{
          let number = "";
          while(i < expression.length && !isNaN(expression[i])){
            number += expression[i];
            i++;
          }
          i--;
          numbers.push(number);
        }
      }
        while(ops.length > 0){
          this.process(numbers, ops[ops.length - 1]);
          ops.pop();
        }
        return numbers[0];
    },
    process(stack, operation){
      let operand2 = parseInt(stack.pop());
      let operand1 = parseInt(stack.pop());
      let value = 0;
      switch(operation){
        case '+': value = operand1 + operand2; break;
        case '-': value = operand1 - operand2; break;
        case '*': value = operand1 * operand2; break;
        case '/': value = operand1 / operand2; break;
      }
      stack.push(value);
/*       return operand2; */
    },
    /* приоритет выполнения операций умножение и деление перед сложением и вычитанием первый вариант */
    getPriority(operation){
      if(operation === "+" || operation === "-")return 1;
      else if(operation === "*" || operation === "/")return 2;
    },
    /* ввод первого числа второй вариант */
    inputNum1(op1){
    /* if(this.operand1 === "")return; */
      this.op1 += op1;
    },
    /* ввод второго числа второй вариант */    
    inputNum2(op2){
    /* if(this.operand2 === "")return; */
      this.op2 += op2;
    },
    inputOp2(operation2){ 
/*      if(this.operation2 === "")return; */
     /*  else  */this.operation2 = operation2; 
      // при вводе с клавиатуры надо вручную добавлять пробелы до и после знака арифметической операции
    },
    calculation2 (operation2) {
      this.error = "";
      switch (operation2) {
        case '+':
        this.add()
          break
        case '-':
          this.substract()
          break
        case '*':
          this.multiply()
          break
        case '/':
          this.divide()
          break
        case 'возведение в степень':
          this.step()
          break
        case 'целочисленное деление':
          this.celdel()
          break
      }
    },
    add () {
      this.total2 = +this.op1 + +this.op2
    },
    substract () {
      this.total2 = +this.op1 - +this.op2
    },
    multiply () {
      this.total2 = +this.op1 * +this.op2
    }, 
    divide () {
      if (this.op2 == 0) this.error = 'На ноль делить нельзя';
      else this.total2 = +this.op1 / +this.op2
    },
    step () {
      this.total2 = +Math.pow(this.op1, this.op2)
    },
    celdel () {
      if (this.op2 == 0) this.error = 'На ноль делить нельзя';
      else this.total2 = +Math.floor(this.op1 / this.op2)
    },
    /* очистить всё */
    allClear(){
      this.input = "";
      this.input1 = "";
      this.input2 = "";
      this.operand1 = "";
      this.operand2 = "";
      this.op1 = "";
      this.op2 = "";
      this.total = "";
      this.total2 = "";
      this.operation = "";
      this.operation2 = "";   
      this.error = "";   
     },
     /* удалить последний введённый символ в первом варианте */
    oneClear(){
      if(this.input[this.input.length - 1] == " ")this.input = this.input.substring(0, this.input.length - 2);
      if(!isNaN(this.input[this.input.length - 1]))this.input = this.input.substring(0, this.input.length - 1);
      this.total = "";
    /* this.calculation(); */
    },
    /* удалить первое число второй вариант */
    oneClear1(){
      this.op1 = "";
      this.total2 = "";
    },
    /* удалить второе число второй вариант */
    oneClear2(){
      this.op2 = "";
      this.total2 = "";
    },
    /* удалить последний введённый символ в первом числе */
    oneClear3(){
      if(this.op1[this.op1.length - 1] == " ")this.op1 = this.op1.substring(0, this.op1.length - 2);
      if(!isNaN(this.op1[this.op1.length - 1]))this.op1 = this.op1.substring(0, this.op1.length - 1);
      this.total2 = "";
    },
        /* удалить последний введённый символ во втором числе */
    oneClear4(){
      if(this.op2[this.op2.length - 1] == " ")this.op2 = this.op2.substring(0, this.op2.length - 2);
      if(!isNaN(this.op2[this.op2.length - 1]))this.op2 = this.op2.substring(0, this.op2.length - 1);
      this.total2 = "";
    },
  }
}
</script>
<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  width: 100wh;
  height: 100vh;
  margin: 0 auto;
  background: rgb(14, 14, 160);
}
h1, h2, p {
  color: white;
  text-align: center;
}
.section {
  width: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  align-content: center;
}
.btn {
  display: block;
  margin: 5px;
  width: 80px;
  height: 40px;
  font-size: 18px;
  border: 1px solid red;
  border-radius: 5px;
  color:  rgb(14, 14, 160);
  cursor: pointer;
}
.btn2 {
  width: 145px;
  height: 50px;
}
.btn3 {
  width: 32%;
  height: 50px;
}
.btn4 {
  width: 46.3%;
  text-align: end;
  padding: 0 10px;
}
.red {
  color: red;
}
.result {
  min-height: 20px;
  text-align: center;
  color: white
}
.label {
  color: white;
  padding-left: 20px;
}
.padding10 {
  padding: 10px;
}
.padding-left-10 {
  padding: 10px 0 0 10px;
}
.error {
  color: white;
  padding-left: 20px;
}
input {
  cursor: pointer;
}
</style>

