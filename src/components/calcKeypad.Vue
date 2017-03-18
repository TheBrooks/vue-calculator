<template>
  <div class="calc-keypad">
    <button class="func" v-on:click="asPercent">%</button>
    <button class="func" v-on:click="toggleSign">+/-</button>
    <button class="func" v-on:click="funcPressed">sqrt</button>
    <button class="func" v-on:click="funcPressed">+</button>
    <br>

    <button class="numeric" v-on:click="digitPressed">7</button>
    <button class="numeric" v-on:click="digitPressed">8</button>
    <button class="numeric" v-on:click="digitPressed">9</button>
    <button class="func" v-on:click="funcPressed">-</button>
    <br>

    <button class="numeric" v-on:click="digitPressed">4</button>
    <button class="numeric" v-on:click="digitPressed">5</button>
    <button class="numeric" v-on:click="digitPressed">6</button>
    <button class="func" v-on:click="funcPressed">x</button>
    <br>

    <button class="numeric" v-on:click="digitPressed">1</button>
    <button class="numeric" v-on:click="digitPressed">2</button>
    <button class="numeric" v-on:click="digitPressed">3</button>
    <button class="func" v-on:click="funcPressed">/</button>
    <br>

    <button class="numeric" v-on:click="digitPressed">0</button>
    <button class="numeric" v-on:click="decimalPressed">.</button>
    <button class="complete" v-on:click="clearPressed">c</button>
    <button class="complete" v-on:click="equalsPressed">=</button>
    <br>
  </div>
</template>

<script>

  export default {
    name: "calc-keypad",
    data : function() {
      return {
        currentVal: "",
      }
    },
    methods: {
      emitNum: function() {
        if(this.currentVal === "" || this.currentVal === "-") {
          return
        }

        this.$emit('number', this.currentVal)
        this.currentVal = ""
      },
      emitFunc: function(func){
        this.$emit('func', func)
      },



      digitPressed: function(digitEvent) {
        var digit = digitEvent.target.innerText
        var curValString = this.currentVal;
        this.currentVal += digit
        this.$emit('typedNum', this.currentVal)
      },
      decimalPressed: function(decEvent) {
      var dec = decEvent.target.innerText
        if (this.currentVal.indexOf(".") < 0) {
          this.currentVal += this.currentVal == "" ? "0" : ""
          this.currentVal += dec
          this.$emit('typedNum', this.currentVal)
        }
      },
      toggleSign: function() {
        if (this.currentVal == ""){
          this.emitFunc('x')
          this.$emit('number', '-1')
          return
        }

        if (this.currentVal.indexOf("-") != -1) {
          this.currentVal = this.currentVal.slice(1)
        }
        else {
          this.currentVal = "-" + this.currentVal
        }
        this.$emit('typedNum', this.currentVal)
      },
      funcPressed: function(funcEvent) {
        var func = funcEvent.target.innerText
        this.emitNum()
        this.emitFunc(func)
      },
      asPercent: function() {
        this.emitNum()
        this.emitFunc("/")
        this.$emit('number', '100')
      },
      equalsPressed: function(){
        this.emitNum()
      },
      clearPressed: function() {
        this.$emit('clear')
        this.currentVal = ""
      },

    }
  }
</script>

<style scoped>

.calc-keypad {

  overflow: hidden;
  border: black 1px solid;
  width: 250px;
}

button {
  width: 50px;
  height: 50px;
  display:  inline-block;
  text-align: center;
  font-size: 16px;
  margin: 4px;
  border: solid 1px black;
}

.numeric {
  background-color: #dbd9d8;
}

.func {
  background-color: #c4c0bd;
}

.complete {
  background-color: #5463eb;
  color: white;
  font-weight: bold;
}
</style>