<template>
  <div class="container">
    <div class="box">
      <div class="result">
        <p class="previous">
          {{calculatorPreviousValue + " " + calculatorOperator}}
          <!-- {{calculatorPreviousValue}} -->
        </p>
        <p class="current">
          {{calculatorValue}}
        </p>
      </div>
      <div class="buttons" :class="[{'yellow-button': ['C', 'AC', '×', '÷', '-', '+', '='].includes(calculatorButton)}, {'big-button': [0].includes(calculatorButton)}]" @click="action(calculatorButton)" v-for="calculatorButton in calculatorButtons" :key="calculatorButton">
        {{calculatorButton}}
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'WebCalculator',
  data(){
    return{
      calculatorValue: "",
      calculatorPreviousValue: '',
      calculatorOperator: "",
      calculatorButtons: ['C', 'AC', '×', '÷', 7, 8, 9, '-', 4, 5, 6, '+', 1, 2, 3, '=', 0, '.'],
      beforeDot: "",
      afterDot: "",
      result: false,
    };
  },
  methods: {
    action(calculatorButton){
      
      if(!isNaN(calculatorButton) || calculatorButton === '.'){
        if(this.result === true) {
          this.calculatorValue = '';
          this.result = false
        }
        if(calculatorButton === '.' && this.calculatorValue.includes('.')) return;
        if(this.calculatorValue.length >= 30) return;
        this.calculatorValue += calculatorButton;
        // this.beforeDot = parseFloat(this.calculatorValue.split('.'[0]))
        // this.afterDot = this.calculatorValue.split('.'[1])
        // if(isNaN(this.beforeDot)){
        //   this.calculatorValue = '';
        // }else{
        //   this.calculatorValue = this.beforeDot.tolocaleString('en', {
        //     maximumFractionDigits: 0
        //   })
        // }
        // if(this.afterDot != ''){
        //   this.calculatorValue = this.beforeDot + '.' + this.afterDot;
        // }else{
        //   this.calculatorValue = this.beforeDot;
        // }

      }
      if(calculatorButton === 'C'){
        this.calculatorValue = '';
        this.calculatorPreviousValue = '';
        this.calculatorOperator = '';
      }
      if(calculatorButton === 'AC'){
        this.calculatorValue = '';
      }
      if(calculatorButton === '×' || calculatorButton === '-' || calculatorButton === '+'){
        if(this.calculatorPreviousValue !== "") this.calculatorValue = this.numbersRound(this.calculatorPreviousValue, this.calculatorOperator, this.calculatorValue)
        this.calculatorOperator = calculatorButton;
        if(calculatorButton == '×') this.calculatorOperator = '*'
        this.calculatorPreviousValue = this.calculatorValue;
        this.calculatorValue = '';
      }
      if(calculatorButton === '÷'){
        if(this.calculatorPreviousValue !== "") this.calculatorValue = this.numbersRound(this.calculatorPreviousValue, this.calculatorOperator, this.calculatorValue)
        this.calculatorOperator = '/'
        
        this.calculatorPreviousValue = this.calculatorValue;
        this.calculatorValue = '';
      }
      if(calculatorButton === '='){
        if(this.calculatorValue == 0 && this.calculatorOperator == '/'){
          this.calculatorValue = 'Nie można dzielić przez 0!';
          this.calculatorPreviousValue = '';
          this.calculatorOperator = ''
          this.result = true
          return;
        } 
        if(this.calculatorValue == ''){
          this.calculatorValue = this.calculatorPreviousValue;
          this.calculatorPreviousValue = '';
          this.calculatorOperator = ''
          return;
        }
        this.calculatorValue = this.numbersRound(this.calculatorPreviousValue, this.calculatorOperator, this.calculatorValue)
        this.calculatorPreviousValue = '';
        this.calculatorOperator = '';
        this.result = true
      }
    },
    numbersRound(previous, operator, current) {
      return Math.round(eval(previous + operator + current) * 100000000) / 100000000 ;
    }
  },
}
</script>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500;600;700&display=swap');
  .container{
    max-width: 440px;
    margin: 50px auto;
    height: 450px;
    /* background-color: rgb(73, 73, 73); */
    background-color: #333;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 10px;
    box-shadow: 2px 2px 20px #888888;
    font-family: 'Open Sans', sans-serif;
  }
  .box{
    padding: 15px 0;
    width: 400px;
    background-color: #000;
    border-radius: 8px;
  }
  .result{
    width: calc(100% - 20px);
    background-color: rgb(73, 73, 73);
    margin: 0 10px;
    padding: 20px;
    color: white;
    text-align: end;
    border-radius: 5px;
  }
  .previous{
    height: 30px;
    font-size: 12px;
    color: rgb(173, 173, 173);
  }
  .current{
    height: 20px;
  }
  .buttons{
    width: calc(84% / 4);
    float: left;
    background-color: #363636;
    margin: 3% 2% 0 2%;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 45px;
    border-radius: 10px;
    color: white;
    cursor: pointer;
    transition: all .1s ease-in-out;
    font-size: 18px;
  }
  .buttons:hover{
    background-color: #474747;
  }
  .yellow-button{
    background-color: #c48105;
    transition: all .1s ease-in-out;
    font-weight: 500;
    font-size: 22px;
  }
  .yellow-button:hover{
    background-color: #d59216;
  }
  .big-button{
    width: 46%;
  }
</style>
