<template>
  <div id="app">
    <div class="calc">
      <calc-result v-bind:digitsOfPersision="this.dop" v-bind:curVal="this.displayMainVal" v-bind:curFunc="this.displayMainFunc" v-bind:altVal="this.displaySideVal"></calc-result>
      <calc-keypad v-on:number="numberEntered" v-on:func="functionEntered" v-on:clear="clear" v-on:typedNum="updateCurVal"></calc-keypad>
    </div>
  </div>
</template>

<script>
import calcResult from "./components/calcResult"
import calcKeypad from "./components/calcKeypad"

export default {
  name: 'app',
  components: {
    calcResult, calcKeypad
  },
  data: function(){
    return {
      mathStack: [],
      curVal: "",
      curFunc: "",
      typeVal: "",
      typing: false,
      dop: 10 //digits of presision
    }
  },
  computed: {
    displayMainVal: function() {
      return this.typing ? this.typeVal : String(this.curVal)
    },
    displaySideVal: function() {
      return this.typing ? String(this.curVal) : ""
    },
    displayMainFunc: function() {
      return this.curFunc
    }
  },
  methods: {
    updateCurVal:function(num) {
      if (this.mathStack.length > 0 && this.mathStack[0].kind === "Num"){
        this.clear()
      }
      this.typeVal = num.substring(0, this.dop)
      this.typing = true
    },
    numberEntered: function(num) {
      if (this.mathStack.length > 0 && this.mathStack[0].kind === "Num"){
        this.clear()
      }
      this.mathStack.unshift({val: num.substring(0, this.dop), kind: "Num"})
      this.typeVal = ""
      this.typing = false

      this.evalStack()
    },
    functionEntered: function(func) {
      if (this.mathStack.length == 0){
        //cannot start with a function
        return
      }
      if (this.mathStack[0].kind == "Func") {
        this.mathStack.shift()
      }
      this.mathStack.unshift({val:func, kind: "Func"})
      this.evalStack()
    },
    clear: function() {
      this.mathStack = []
      this.curVal = ""
      this.typeVal = ""
      this.curFunc = ""
    },
    //so far this will only evaluate for 0 or 1 expression
    evalStack: function(){
      if (this.mathStack.length == 0){
        //set current val
        return
      }
      if (this.mathStack.length == 1){
        this.curVal = this.mathStack[0].val
        return
      }

      else if (this.mathStack[0].kind == "Func"){
        var topFunc = this.mathStack.shift()
        var topFuncVal = topFunc.val
        var operand1 = this.mathStack.shift()
        var outcome = Number(operand1.val)

        switch(topFuncVal) {
          case "sqrt":
              topFuncVal = ""
              outcome = Math.sqrt(outcome)

              this.mathStack.unshift({val: outcome, kind: "Num"})
            break
          default :
              this.mathStack.unshift(operand1)
              this.mathStack.unshift(topFunc)
            break
        }

        this.curFunc = topFuncVal
        this.curVal = outcome
      }
      else{
        var operand2 = Number(this.mathStack.shift().val)
        var op = this.mathStack.shift().val
        var operand1 = Number(this.mathStack.shift().val)

        var outcome = ""
        switch(op) {
          case "+":
            outcome = operand1 + operand2;
            break
          case "-":
            outcome = operand1 - operand2;
            break
          case "x":
            outcome = operand1 * operand2;
            break
          case "/":
            outcome = operand1 / operand2;
            break
          default :
            break
        }

        this.mathStack.unshift({val: outcome, kind:"Num"})
        this.curVal = outcome
        this.curFunc = ""
      }
    },
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
.calc {
  display: inline-block;
  margin-left: auto;
  margin-right: auto;
}
</style>
