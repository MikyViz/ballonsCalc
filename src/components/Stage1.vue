<script setup>
import { ref, watch } from 'vue'

const emit = defineEmits(['deviation-percentage', 'styles-num'])


// const imgUrl = ref('https://i.pinimg.com/564x/a0/af/fa/a0affa452658c0766686f75dcbfb745f.jpg')
const imgUrl = ref('../../public/placeholder-300x200.png')
const imageRef = ref(null)

const onFileChange = (e) => {
  const file = e.target.files[0]
  imgUrl.value = URL.createObjectURL(file)
}

const selectedPercent = ref(10);
const stylesNum = ref(0);

watch(() => selectedPercent.value, (newVal) => {
  emit('deviation-percentage', newVal);
});

watch(() => stylesNum.value, (newVal) => {
  emit('styles-num', newVal);
});
</script>
    
<template>
  <div class="wrapper1">
    <!-- <div class="img">
      <img :src="imgUrl" />
    </div>
    <h2>שלב 1 פרטים</h2>
    <form action="" method="get">
      <input type="text" name="" id="" placeholder="שם העיצוב" />
      <input type="text" name="" id="" v-model="stylesNum" placeholder="כמות" />
      <input type="file" accept="image/*" @change="onFileChange"/>
    </form>
    <div class="percent-slider">
      <label for="percentSlider">אחוז סטייה</label>
      <input type="range" id="percentSlider" min="0" max="100" step="1" v-model="selectedPercent" />
      <div id="selectedPercent">{{ selectedPercent }}%</div>
    </div> -->


    <div class="calc-img-wrapper">
        <div class="calc-img">
          <img :src="imgUrl" />
        </div>
      </div>

        <div class="calc-stage calc-stage1">
            <h2>שלב 1 פרטים</h2>
            <form class="calc-form" method="get">
              <input type="text" name="" id="" placeholder="שם העיצוב" />
              <input type="text" name="" id="" v-model="stylesNum" placeholder="כמות" />
              <input type="file" accept="image/*" @change="onFileChange"/>
            </form>
        </div>
        <div class="percent-slider">
          <label for="percentSlider">אחוז סטייה</label>
          <input type="range" id="percentSlider" min="0" max="100" step="1" v-model="selectedPercent" />
          <div id="selectedPercent">{{ selectedPercent }}%
          </div>
        </div>

  </div>
</template>

<style scoped>

.wrapper1 {
    width: 800px;
    margin: auto;
}

.info {
  height: 50%;
  width: 50%;
}

.percent-slider {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.percent-slider input[type='range'] {
  width: 100%;
}

#selectedPercent {
  top: -30px;
  left: 0;
  background-color: #fcea92;
  border: 2px solid #fcea92;
}


</style>