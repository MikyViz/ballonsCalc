    <script setup>
import { ref, defineProps, computed } from 'vue'

const props = defineProps({
  MathMenu: Array,
  WorkMenu: Array,
  GenMenu: Array,
  discountPercent: Number,
  deviationPercentage: Number,
  stylesNum: Number,
  summaryCalc: Boolean
})
const sumPrice = computed(() => {
  let sum = 0;
  let Worksum = 0;
  const calcObj = {};
  let profitInPrcnt = 0;
  let profitInNis = 0;
  let expenses = 0;
  let sumForPrcntCalc = 0;
  let profitAfterDiscountInPrcnt = 0;
  let profitAfterDiscountInNis = 0;
  if (props.MathMenu) {
    for (let i = 0; i < props.MathMenu.length; i++) {
      if(!props.MathMenu[i].type.ballonsInCube)
      expenses += props.MathMenu[i].price * props.MathMenu[i].quantity
    else
    expenses += (props.MathMenu[i].price / props.MathMenu[i].type.ballonsInCube) * props.MathMenu[i].quantity
    }
    if(props.deviationPercentage)
      expenses += expenses*(props.deviationPercentage/100)
    
  }
  if (props.WorkMenu) {
    for (let i = 0; i < props.WorkMenu.length; i++) {
      Worksum += props.WorkMenu[i].price * props.WorkMenu[i].quantity ;
    }
  }
  
  if (props.stylesNum){
    expenses = expenses*props.stylesNum;
    Worksum = Worksum*props.stylesNum;
  }

  if (props.GenMenu) {
    for (let i = 0; i < props.GenMenu.length; i++) {
      if (props.GenMenu[i].type === 'זמן עבודה כללי') {
        Worksum += props.GenMenu[i].price * props.GenMenu[i].quantity;
      } else {
        expenses += (props.GenMenu[i].price * props.GenMenu[i].quantity) + ((props.GenMenu[i].price * props.GenMenu[i].quantity)*(props.deviationPercentage/100))
      }
      
    }
  }
  // if (props.stylesNum){
  //   expenses = expenses*props.stylesNum;
  //   Worksum = Worksum*props.stylesNum;
  // }
  

sum = Worksum + expenses;
profitInPrcnt = (Worksum/sum)*100;

sumForPrcntCalc = sum * (1 - props.discountPercent/100);

profitAfterDiscountInPrcnt = (((sumForPrcntCalc-expenses))/sumForPrcntCalc) * 100;

profitAfterDiscountInNis = sumForPrcntCalc - expenses;

profitInNis = sum - expenses;


calcObj.expenses = expenses;
calcObj.Worksum = Worksum;
calcObj.sum = sum;
calcObj.profitInPrcnt = profitInPrcnt;
calcObj.profitInNis = profitInNis;
calcObj.sumForPrcntCalc = sumForPrcntCalc;
calcObj.profitAfterDiscountInPrcnt = profitAfterDiscountInPrcnt;
calcObj.profitAfterDiscountInNis = profitAfterDiscountInNis;

  return calcObj
})
</script>
    
<template>
  <div class="sum-wrapper">
    <h2> {{`חלון סיכום ${props.GenMenu ? " " : "לא"} כולל עלויות כלליות`}} </h2>
    <div class="container">
      <div class="general-sum">
        {{ ` סך הכל ${props.GenMenu ? " " : "ליחידה"} :₪${sumPrice.sumForPrcntCalc.toFixed(2)}` }}
      </div>
      <div class="other-profits-wrapper">
          <div class="other-profits">
            <div>
              {{ `רווח באחוזים: %${sumPrice.profitInPrcnt ? sumPrice.profitInPrcnt.toFixed(2) : 0} ` }}
            </div>
            <div>
              {{ `רווח באחוזים אחרי הנחה: %${sumPrice.profitAfterDiscountInPrcnt ? sumPrice.profitAfterDiscountInPrcnt.toFixed(2) : 0} `}}
            </div>
          </div>
          <div class="other-profits">
            <div>
              {{ `רווח בש"ח: ₪${sumPrice.profitInNis ? sumPrice.profitInNis : 0} ` }}
            </div>
            <div>
              {{ ` רווח בש"ח אחרי הנחה: ₪${sumPrice.profitAfterDiscountInNis ? sumPrice.profitAfterDiscountInNis.toFixed(2) : 0}` }}
            </div>
          </div>
       </div> 
    </div>
  </div>
</template>

<style scoped>

.sum-wrapper {
  width: 100%;
  max-width: 800px;
  background-color: #D9D9D9;
  /* margin-bottom: 20px; */
  border: 2px solid #000;
  border-radius: 10px;
  padding: 20px;
  margin: 20px auto;
}

.sum-wrapper h2 {
  font-size: 18px;
  display: flex;
  justify-content: center;
  text-align: center;
  color: #000;
  font-family: 'Montserrat-Medium',Arial,sans-serif;
}

.container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  border-radius: 10px;
  direction: rtl;
}

.general-sum {
  font-size: 26px;
  font-weight: bold;
  color: #E81758;
}

.other-profits-wrapper {
  display: flex;
  justify-content: space-evenly;
  width: 100%;
}
.other-profits {
  font-size: 18px;
}

@media (max-width: 1024px) {
.other-profits {
  font-size: 12px;
}
.other-profits-wrapper {
  flex-direction: column;
  padding-right: 50px;
}
} 

</style>
