<script setup>
import { ref} from 'vue'
import Stage1 from './components/Stage1.vue'
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
const projectName = ref(10)
const imgUrl = ref(10)

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
const updateProjectName = (newProjectName) => {
  projectName.value = newProjectName
}
const updateImgUrl = (newImgUrl) => {
  imgUrl.value = newImgUrl
}

// 🫎🫎🫎🫎🫎🫎🫎 FETCH ЗАПРОС

const save = async ()=>{
  try {
    const myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InZpemVub3Zza3lAZ21haWwuY29tIiwiaWQiOjEsImV4cCI6MTcxMzQ2NzUwMywiaWF0IjoxNzA4Mjg3MTA3fQ.AFKJGF5bHA-_7cNu0UpDI6-wRhTQiO58uBSzw8tfhaE");
    
    const projectStageOne = JSON.stringify({
      "name": projectName.value,
      "stylesNum": stylesNum.value,
      "imgPath": imgUrl.value,
      "deviationPercentage":  deviationPercentage.value,
      "discountPercent": discountPercent.value,
      "UserId": "1"
    });
    const StageOneRequestOptions = {
  method: "POST",
  headers: myHeaders,
  body: projectStageOne,
  redirect: "follow"
};
const response = await fetch("http://localhost:3000/project/saveNewProject1", StageOneRequestOptions)
const stageOneData = await response.json();
const projectId = stageOneData.id; // 👈Your Project id bro... !!!!

const MathMenuFetch = ()=>{
 
 MathMenu.value.forEach(async (value, key)=> {
  const response = await fetch("http://localhost:3000/project/saveNewProject2", {
    method: "POST",
  headers: myHeaders,
  body: JSON.stringify({
    "projectId": projectId,
      "category": value.category,
      "subcategoty": value.subcategoty,
      "type": value.type.name,
      "ballonsInCube": value.type.ballonsInCube ? value.type.ballonsInCube : null,
      "quantity":  value.quantity,
      "price": value.price,}),
  redirect: "follow"
  })
  const data = await response.json();
  console.log(data);

  });
}

const WorkMenuFetch = ()=>{
 
 WorkMenu.value.forEach(async (value, key)=> {
  const response = await fetch("http://localhost:3000/project/saveNewProject2", {
    method: "POST",
  headers: myHeaders,
  body: JSON.stringify({
    "projectId": projectId,
      "category": value.category,
      "subcategoty": value.subcategoty,
      "type": value.type.name,
      "ballonsInCube": value.type.ballonsInCube ? value.type.ballonsInCube : null,
      "quantity":  value.quantity,
      "price": value.price,}),
  redirect: "follow"
  })
  const data = await response.json();
  console.log(data);

  });
}

MathMenuFetch();

console.log("👉 ", projectId, " 👈");
console.log(stageOneData);
} catch (error) {
    console.log("🤮  ",error);
  }
}

</script>

<template>
  <div class="wrapper">
    <h1>מחשבון תמחור</h1>

    <Stage1 @deviation-percentage="updateDeviationPercentage" @styles-num="updateStylesNum" @project-name="updateProjectName" @img-url="updateImgUrl" />
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
    <div class="btns">
      <input type="button" value="לשמירת פרויקט" class="btn" @click="save" />

      <a
        href="https://bsch.co.il/uncategorized-en/%d7%9e%d7%97%d7%a9%d7%91%d7%95%d7%9f-%d7%aa%d7%9e%d7%97%d7%95%d7%a8/"
        target="_blank"
        class="btn"
      >
        להסבר על המחשבון לחץ כאן
      </a>
    </div>
  </div>
</template>

<style scoped>
.material-symbols-outlined {
  display: flex;
  align-items: center;
  flex-direction: column;
  font-variation-settings: 'FILL' 0, 'wght' 400, 'GRAD' 0, 'opsz' 24;
}

.material-symbols-outlined span {
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

.btns {
  display: flex;
  align-items: stretch;
  flex-direction: column;
}
.btn {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 20px;

  padding: 20px 15px;
  background-color: #e81758;
  color: #000;
  border-radius: 30px;
  transition: transform 0.2s ease-in-out;
}

.btn:hover {
  transform: scale(1.1);
}

@media (max-width: 376px) {
  .btn {
    font-weight: 400;
    padding: 15px 10px;
  }
}
</style>
