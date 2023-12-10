<script setup>
import { ref, toRefs, onMounted, onBeforeUnmount } from 'vue'
import Stage1 from './components/Stage1.vue'
// import Stage2pseudoDropDown from './components/Stage2pseudoDropDown.vue'
import Stage2 from './components/Stage2.vue'
import Stage3 from './components/Stage3.vue'
import Stage4 from './components/Stage4.vue'
import commonSum from './components/commonSum.vue'

const MathMenu = ref(null)
const WorkMenu = ref(null)
const GenMenu = ref(null)
const discountPercent = ref(0)
const deviationPercentage = ref(10)
const stylesNum = ref(null)

const updateMatherialMenu = (newMenu) => {
  MathMenu.value = newMenu
}
const updateWorkMenu = (newMenu) => {
  WorkMenu.value = newMenu
}
const updateGeneralCostsMenu = (newMenu) => {
  GenMenu.value = newMenu
}
const updateDeviationPercentage = (newPercentage) => {
  deviationPercentage.value = newPercentage
}
const updateStylesNum = (newStylesNum) => {
  stylesNum.value = newStylesNum
}
</script>

<template>
  <div class="wrapper">
    <h1>מחשבון תמחור</h1>

    <Stage1 @deviation-percentage="updateDeviationPercentage" @styles-num="updateStylesNum" />
    <Stage2 @update-menu="updateMatherialMenu" />
    <Stage3 @update-menu="updateWorkMenu" />
    <Stage4 @update-menu="updateGeneralCostsMenu" />
    <commonSum
      :MathMenu="MathMenu"
      :WorkMenu="WorkMenu"
      :discountPercent="discountPercent"
      :deviationPercentage="deviationPercentage"
      :summaryCalc="false"
    />
    <commonSum
      :MathMenu="MathMenu"
      :WorkMenu="WorkMenu"
      :GenMenu="GenMenu"
      :discountPercent="discountPercent"
      :deviationPercentage="deviationPercentage"
      :stylesNum="stylesNum"
      :summaryCalc="true"
    />

    <div class="discount-slider">
      <label for="discountSlider">אחוז הנחה</label>
      <input
        type="range"
        id="discountSlider"
        min="0"
        max="100"
        step="1"
        v-model="discountPercent"
      />
      <div id="discountPercent">{{ discountPercent }}%</div>
    </div>
    <a href="https://bsch.co.il/uncategorized-en/%d7%9e%d7%97%d7%a9%d7%91%d7%95%d7%9f-%d7%aa%d7%9e%d7%97%d7%95%d7%a8/"
   target="_blank" >
   להסבר על המחשבון לחץ כאן
</a>
  </div>
</template>

<style scoped>

.material-symbols-outlined {
  display: flex;
  align-items: center;
  flex-direction: column;
  font-variation-settings:
  'FILL' 0,
  'wght' 400,
  'GRAD' 0,
  'opsz' 24
}

.material-symbols-outlined span{
  font-family: sans-serif;
  font-size: 20px;
}
.wrapper h1 {
  text-align: center;
  color: #37474f;
  font-size: 26px;
  font-weight: bold;
  margin-bottom: 20px;
  font-family: 'Montserrat-Medium', Arial, sans-serif;
}


.wrapper {
  padding: 20px;
}

.discount-slider {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 800px;
  margin: 20px auto;
}

.discount-slider input[type='range'] {
  width: 100%;
}

a {

  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  font-weight: bold;

  padding: 20px 15px;
  background-color: #e81758;
  color: #000;
  border-radius: 30px;  
  transition: transform 0.2s ease-in-out;
}


a:hover {
  transform: scale(1.1);
}

@media (max-width: 376px) {
  a{
    font-weight: 400;
    padding: 15px 10px;
  }
}
</style>
