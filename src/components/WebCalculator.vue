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
      <div class="buttons" :class="[{'yellow-button': ['C', 'AC', '*', '/', '-', '+', '='].includes(calculatorButton)}, {'big-button': [0].includes(calculatorButton)}]" @click="action(calculatorButton)" v-for="calculatorButton in calculatorButtons" :key="calculatorButton">
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
      calculatorButtons: ['C', 'AC', '*', '/', 7, 8, 9, '-', 4, 5, 6, '+', 1, 2, 3, '=', 0, '.'],
    };
  },
  methods: {
    action(calculatorButton){
      if(!isNaN(calculatorButton) || calculatorButton === '.'){
        if(calculatorButton === '.' && this.calculatorValue.includes('.')) return;
        this.calculatorValue += calculatorButton;
      }
      if(calculatorButton === 'C'){
        this.calculatorValue = '';
        this.calculatorPreviousValue = '';
        this.calculatorOperator = '';
      }
      if(calculatorButton === 'AC'){
        this.calculatorValue = '';
      }
      if(calculatorButton === '*' || calculatorButton === '-' || calculatorButton === '+'){
        this.calculatorOperator = calculatorButton;
        if(this.calculatorPreviousValue !== ""){
          this.calculatorValue = eval(this.calculatorPreviousValue + this.calculatorOperator + this.calculatorValue) 
        }
        this.calculatorPreviousValue = this.calculatorValue;
        this.calculatorValue = '';
      }
      if(calculatorButton === '/'){
        if(this.calculatorValue == 0){
          this.calculatorValue = 'Nie można dzielić przez 0!';
          return;
        } 
        this.calculatorOperator = calculatorButton;
        if(this.calculatorPreviousValue !== ""){
          this.calculatorValue = eval(this.calculatorPreviousValue + this.calculatorOperator + this.calculatorValue) 
        }
        this.calculatorPreviousValue = this.calculatorValue;
        this.calculatorValue = '';
      }
      if(calculatorButton === '='){
        if(this.calculatorValue == 0 && this.calculatorOperator == '/'){
          console.log(this.calculatorOperator)
          this.calculatorValue = 'Nie można dzielić przez 0!';
          return;
        } 
        if(this.calculatorValue == ''){
          this.calculatorValue = this.calculatorPreviousValue;
          this.calculatorPreviousValue = '';
          this.calculatorOperator = ''
          return;
        }
        this.calculatorValue = eval(this.calculatorPreviousValue + this.calculatorOperator + this.calculatorValue);
        this.calculatorPreviousValue = '';
        this.calculatorOperator = '';
      }
    }
  },
}
</script>

<style scoped>
  .container{
    max-width: 440px;
    margin: 50px auto;
    height: 450px;
    background-color: rgb(73, 73, 73);
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 10px;
    box-shadow: 2px 2px 20px #888888;
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
    background-color: rgb(54, 54, 54);
    margin: 3% 2% 0 2%;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 45px;
    border-radius: 10px;
    color: white;
  }
  .yellow-button{
    background-color: rgb(196, 129, 5);
  }
  .big-button{
    width: 46%;
  }
</style>
