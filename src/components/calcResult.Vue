<template>
  <div class="calc-result">
    <h1 class="cur-val">{{curVal | truncate(10)}}</h1>  
    <p class="alt-val">{{altVal }}</p>
    <p class="cur-func">{{curFunc}}</p>
    <br>
  </div>
</template>

<script>
  export default {
    name: "calc-result",
    props: {
      curVal: {
        type: String,
        default: "0"
      },
      curFunc: {
        type: String,
        default: ""
      },
      altVal: {
        type: String,
        default: ""
      },
      digitsOfPresision: {
        type: Number,
        default: 8
      }
    },
    data:  function() {
      return {
        dop: this.digitsOfPresision
      }
      
    },
    filters: {
      truncate: function(str, dop) {
        var absVal = Math.abs(Number(str))
        if (absVal != 0 && (absVal > Math.pow(10, dop) ||
          absVal < Number(Math.pow(0.1, dop-2).toFixed(dop-2)))){
          console.log('to toExponential')
          return Number(str).toExponential()
        }
        return str.substring(0, dop);
      }
    }
  }
</script>

<style scoped>
.calc-result {
  border: black 1px solid;
  height: 70px;
  width: 250px;
  background-color: #ded9d6;
  overflow: hidden;
  position: relative;
}

.cur-val {
  margin-right: 5px;
  float: right;
  text-align: right;
}
.alt-val{
  position: absolute;
  margin-top: 0px;
  top: 5px;
  left: 5px;
  font-size: 18px;
}
.cur-func{
  position: absolute;
  margin-bottom: 0px;
  bottom: 5px;
  left: 5px;
  font-size: 18px;
}
</style>