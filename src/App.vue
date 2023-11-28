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

    const screenWidth = ref(window.innerWidth);

    const handleResize = () => {
      screenWidth.value = window.innerWidth;
    };

    onMounted(() => {
      window.addEventListener('resize', handleResize);
    });

    onBeforeUnmount(() => {
      window.removeEventListener('resize', handleResize);
    });

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
    <a
      href="https://bsch.co.il/maslulim/?mepr-unauth-page=17697&redirect_to=%2Funcategorized-en%2F%25d7%259e%25d7%2597%25d7%25a9%25d7%2591%25d7%2595%25d7%259f-%25d7%25aa%25d7%259e%25d7%2597%25d7%2595%25d7%25a8%2F"
      >{{screenWidth >= 1024 ? 'להסבר על המחשבון לחץ כאן' : "¿?"}}</a
    >
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
  </div>
</template>

<style scoped>
.wrapper h1 {
  text-align: center;
  color: #37474f;
  font-size: 26px;
  font-weight: bold;
  margin-bottom: 20px;
  font-family: 'Montserrat-Medium', Arial, sans-serif;
}

a {
  display: inline-block;
  padding: 20px 15px;
  background-color: #e81758;
  color: #000;
  border-radius: 30px;
  font-weight: bolder;
  transition: transform 0.2s ease-in-out;
  position: fixed; /* Добавлено свойство position */
  top: 13px;
  left: 14px;
  z-index: 1000; /* Вы можете настроить z-index в зависимости от вашего дизайна */
}

a:hover {
  transform: scale(1.1);
}

@media (max-width: 800px) {
  a {
  padding: 15px 10px;
  font-weight: 500;
}
}

.wrapper {
  padding: 20px;
  /* display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  width: 100%; */
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

</style>
