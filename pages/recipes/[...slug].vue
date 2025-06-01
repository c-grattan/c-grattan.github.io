<template>
  <BContainer>
    <BRow>
      <BCol offsetMd="6">
        <div class="d-flex justify-content-between">
          <p class="h1">{{ recipe.recipeTitle }}</p>
          <BButton v-if="recipe.temperatures" v-model="useCelcius" v-on:click="useCelcius = !useCelcius">{{ useCelcius ? 'Celcius' : 'Fahrenheit' }}</BButton>
        </div>
        <hr/>
      </BCol>
    </BRow>
    <BRow>
      <BCol md="6" class="sticky-md-top h-100" style="top: 10%;">
        <ConvertableLabel
          v-for="(ingredient, index) in recipe.ingredients"
          v-bind:key="index"
          :ingredient="ingredient"
          :factor="factor"
          :serves="recipe.serves"
          :convertVolume="convertVolume"
          :convertMass="convertMass"
        />
        <span class="d-flex align-items-center justify-content-between mt-3">
          <p class="mb-0">Serves</p><BFormInput class="ms-1 w-auto" type="number" min=1 v-model="factor" />
        </span>
      </BCol>
      <BCol md="6" class="mt-3 mt-md-0">
        <ContentRenderer :value="recipeData as any" :data="markdownData" />
      </BCol>
    </BRow>
  </BContainer>
</template>
<script setup lang="ts">
import configureMeasurements from 'convert-units';
import volume from 'convert-units/definitions/volume';
import mass from 'convert-units/definitions/mass';

const convertVolume = configureMeasurements({volume});
const convertMass = configureMeasurements({mass});

export type Ingredient = {
  name: string,
  amount: number,
  unit: string
  approx?: number
  alternative?: Ingredient
}

type Recipe = {
  recipeTitle: string;
  ingredients: Ingredient[];
  serves: number;
  temperatures?: number[]
};

const route = useRoute()
const { data: recipeData } = await useAsyncData(route.path, () => {
  return queryCollection('content').path(route.path).first()
});

const recipe = computed<Recipe>(() => {
  return recipeData.value
    ? recipeData.value.meta as Recipe
    : {
        recipeTitle: '',
        ingredients: [],
        serves: 1
      }
});

const factor = ref(recipe.value.serves);

const useCelcius = ref(true);
function convertToFahrenheit(celcius: number) {
  switch(celcius) {
    case 180:
      return 360
    case 200:
      return 400
    case 220:
      return 430
    default:
      return celcius * 1.8 + 32;
  }
}
const markdownData = computed(() => {
  return useCelcius.value ? {
    temp: recipe.value.temperatures?.map((temp) => {
      return temp + '℃';
    })
  } : {
    temp: recipe.value.temperatures?.map((temp) => {
      return convertToFahrenheit(temp) + '℉';
    })
  };
});
</script>