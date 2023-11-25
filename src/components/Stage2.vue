    <script setup>
import { ref, reactive, computed } from 'vue'
const selectedCategory = ref(null)
const selectedSubcategory = ref(null)
const selectedType = ref(null)
const selectedTypes = reactive([])
const quantity = ref(0)
const price = ref(0)
const selectedMenus = reactive([])

const emit = defineEmits(['update-menu'])

const filters = reactive([
  {
    category: 'בלונים',
    subCategory: [
      {
        name: 'בלוני לטקס',
        type: [
          { name: `5"` },
          { name: `9"/10"` },
          { name: `11"/12"` },
          { name: `16"` },
          { name: `18"/19"` },
          { name: `24"` },
          { name: `3"/36"` },
          { name: `160` },
          { name: `260` },
          { name: `350` },
          { name: `646` },
          { name: `לינק 6"` },
          { name: `לינק 12"` },
          { name: `לטקס שונות/מיוחדים` }
        ]
      },
      {
        name: 'בלוני מיילר',
        type: [
          { name: `4"` },
          { name: `7"-9"` },
          { name: `14"-16"` },
          { name: `18"-22"` },
          { name: `28"-29"` },
          { name: `30"-32"` },
          { name: `34"-36"` },
          { name: `38"-40"` },
          { name: ` '+40"` },
          { name: `שונות` }
        ]
      },
      {
        name: `אקווה\בובו`,
        type: [
          { name: `4"` },
          { name: `14"` },
          { name: `18"/20"` },
          { name: `22"/24"` },
          { name: `36"` }
        ]
      },
      {
        name: 'באבל',
        type: [
          { name: `שקוף דקו 20"` },
          { name: `שקוף דקו 24"` },
          { name: `סינגל באבל "22` },
          { name: `דאבל באבל` }
        ]
      },
      { name: 'אורבז', type: [{ name: `מיני` }, { name: `רגיל` }, { name: `ג'אמבו` }] }
    ]
  },
  {
    category: 'הליום',
    subCategory: [
      {
        name: 'בלוני לטקס',
        type: [
          { name: `9"`, ballonsInCube: 143 },
          { name: `11"`, ballonsInCube: 66 },
          { name: `16"`, ballonsInCube: 23 },
          { name: `18"/19"`, ballonsInCube: 17 },
          { name: `24"`, ballonsInCube: 7 },
          { name: `3"/36"`, ballonsInCube: 4 },
          { name: `לינק 12"`, ballonsInCube: 60 }
        ]
      },
      {
        name: 'בלוני מיילר',
        type: [
          { name: `18"`, ballonsInCube: 71 },
          { name: `20"-22"`, ballonsInCube: 50 },
          { name: `28"-29"`, ballonsInCube: 19 },
          { name: `30"-32"`, ballonsInCube: 17 },
          { name: `34"-36"`, ballonsInCube: 16 },
          { name: `38"-40"`, ballonsInCube: 15 },
          { name: ` '+40"`, ballonsInCube: 13 }
        ]
      },
      {
        name: `אקווה\ובוב`,
        type: [
          { name: `14"`, ballonsInCube: 110 },
          { name: `18"/20"`, ballonsInCube: 50 },
          { name: `22"/24"`, ballonsInCube: 25 },
          { name: `28"`, ballonsInCube: 6 },
          { name: `36"`, ballonsInCube: 4 }
        ]
      },
      {
        name: 'באבל',
        type: [
          { name: `שקוף דקו 20"`, ballonsInCube: 50 },
          { name: `שקוף דקו 24"`, ballonsInCube: 23 },
          { name: `סינגל באבל "22"`, ballonsInCube: 37 },
          { name: `דאבל באבל`, ballonsInCube: 29 }
        ]
      },
      {
        name: 'אורבז',
        type: [
          { name: `רגיל`, ballonsInCube: 32 },
          { name: `ג'אמבו`, ballonsInCube: 11 }
        ]
      }
    ]
  },
  {
    category: 'ציוד',
    subCategory: [
      {
        name: 'דבקים',
        type: [
          { name: `בלון בונד` },
          { name: `סטרצ'י` },
          { name: `גלו גוטס` },
          { name: `חבקים` },
          { name: `דבק אחר` }
        ]
      },
      { name: `בדים\םסרטי`, type: [{ name: `בד טול` }, { name: `סרט סטן` }, { name: `סרט מתנה` }] },
      {
        name: `סטנדים\תקונסתרוקציו`,
        type: [
          { name: `סטנד שולחן` },
          { name: `סטנד רצפה` },
          { name: `קשתות` },
          { name: `לוח קאפה` },
          { name: `לוח פי וי סי` },
          { name: `לוח פרספקט` },
          { name: `שונות` }
        ]
      },
      {
        name: `שונות`,
        type: [
          { name: `אקססוריז` },
          { name: `ספריי צבע` },
          { name: `בלון שיין/היי שיין` },
          { name: `הייפלוט` },
          { name: `חוט דיג` },
          { name: `מוט גמיש` },
          { name: `מדבקות ויניל` },
          { name: `שונות` }
        ]
      }
    ]
  }
  // {
  //   category: 'שעות עבודה',
  //   subCategory: [{ name: 'עבודה בסיסית' }, { name: 'עבודה אמנותית' }, { name: 'עבודה מורכבת' }]
  // },
  // {
  //   category: 'עלויות כלליות',
  //   subCategory: [
  //     { name: 'זמן עבודה כללי' },
  //     { name: 'משלוח' },
  //     { name: 'עובדים' },
  //     { name: 'שונות' }
  //   ]
  // }
])
const selectedCategorySubcategories = computed(() => {
  // selectedCategorySubcategories in return contains array with names of subcategorys
  const category = filters.find((item) => item.category === selectedCategory.value)
  return category ? category.subCategory.map((sub) => sub.name) : []
})

const selectedSubcategoryTypes = computed(() => {
  // like selectedCategorySubcategories 👆 just for containig of types
  const category = filters.find((item) => item.category === selectedCategory.value)
  if (category) {
    const subcategory = category.subCategory.find((sub) => sub.name === selectedSubcategory.value)
    return subcategory ? subcategory.type : []
  }
  return []
})

const addNewMenu = () => {
  // create info for menu

  if (selectedCategory && selectedSubcategory) {
    const menu = {
      category: selectedCategory.value,
      subcategory: selectedSubcategory.value,
      quantity: quantity.value,
      price: price.value
    }
    if (selectedType.value) {
      menu.type = selectedType.value
      selectedTypes.push(selectedType.value)
      selectedType.value = null
    }
    selectedMenus.push(menu)
    quantity.value = 0
    price.value = 0
  }
}
emit('update-menu', selectedMenus)

// const addNewMenuSub = () => {
//   if (!selectedSubcategoryTypes.value) {
//     addNewMenu()
//   }
// }

selectedType.value = null
</script>
    
<template>
  <div class="calc-stage calc-stage2">
    <h2>שלב 2 בחר חומרים</h2>
    <div class="strawberry-pink-bg-wrapper">
      <div class="strawberry-pink-bg-select-wrapper">
        <select v-model="selectedCategory" class="strawberry-pink-bg">
          <option disabled value="" class="rose-red-text">בחר אחת מהקטגוריות</option>
          <option
            v-for="(category, index) in filters"
            :key="index"
            :value="category.category"
            class="rose-red-text"
          >
            {{ category.category }}
          </option>
        </select>

        <select
          v-model="selectedSubcategory"
          v-if="selectedCategory"
          @change="addNewMenuSub"
          class="strawberry-pink-bg"
        >
          <option disabled value="" class="rose-red-text">בחר תתקטגוריה</option>
          <option
            v-for="(subcategory, index) in selectedCategorySubcategories"
            :key="index"
            :value="subcategory"
            class="rose-red-text"
          >
            {{ subcategory }}
          </option>
        </select>
      </div>
      <select
        v-model="selectedType"
        v-if="selectedSubcategory"
        @change="addNewMenu"
        class="strawberry-pink-bg"
      >
        <option disabled value="" class="rose-red-text">בחר סוג</option>
        <option
          v-for="(type, index) in selectedSubcategoryTypes"
          :key="index"
          :value="type"
          class="rose-red-text"
        >
          {{ type.name }}
        </option>
      </select>

      <div v-for="(menu, index) in selectedMenus" :key="index" class="additional-fields">
        <div class="menu-container">
          <!-- <button
            @click="
              () => {
                selectedMenus.splice(index, 1)
              }
            "
          >
            &#215;
          </button>
          <div class="additional-info">
            {{ menu.category }} {{ menu.subcategory }} {{ menu.type.name }}
          </div> -->

          <div class="remove-button-wrapper">
                <button class="remove-button"
                  @click="
                    () => {
                      selectedMenus.splice(index, 1)
                    }
                  "
                >
                  &#215;
                </button>
                <div class="additional-info">
                  {{ menu.category }} {{ menu.subcategory }} {{ menu.type.name }}
                </div>
          </div>

          <div class="input-field">
            <label for="quantity">כמות</label>
            <input
              id="quantity"
              type="number"
              v-model="menu.quantity"
              placeholder="כמות"
              class="strawberry-pink-bg"
            />
          </div>
          <div class="input-field">
            <label for="price">מחיר ליחידה</label>
            <input
              id="price"
              type="number"
              v-model="menu.price"
              placeholder="מחיר ליחידה"
              class="strawberry-pink-bg"
            />
          </div>
          <select
            v-model="selectedTypes[index]"
            @change="(event) => (menu.type = event.target.value)"
          >
            <option disabled value="" class="rose-red-text">שנה סוג</option>
            <option
              v-for="(type, index) in selectedSubcategoryTypes"
              :key="index"
              :value="type"
              class="rose-red-text"
            >
              {{ type.name }}
            </option>
          </select>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>
</style>
