<template>
    <span class="d-flex align-items-center justify-content-between">
        <p class="mb-0 me-1">{{ labelText }}</p>
        <div class="d-flex">
            <BFormSelect v-if="currentIngredient.unit.length > 0" class="w-auto" v-model="currentUnit" :options="unitOptions" />
            <BButton v-if="ingredient.alternative" v-on:click="() => useAlternativeIngredient = !useAlternativeIngredient">Toggle</BButton>
        </div>
    </span>
</template>
<script setup lang="ts">
import type { Converter } from 'convert-units';
import type { MassSystems, MassUnits } from 'convert-units/definitions/mass';
import type { VolumeSystems, VolumeUnits } from 'convert-units/definitions/volume';
import type { Ingredient } from '~/pages/recipes/[...slug].vue';

const props = defineProps<{
    ingredient: Ingredient;
    factor: number;
    serves: number;
    convertVolume: (value?: number) => Converter<"volume", VolumeSystems, VolumeUnits>;
    convertMass: (value?: number) => Converter<"mass", MassSystems, MassUnits>;
}>();

const useAlternativeIngredient = ref(false);

const currentUnit = ref(props.ingredient.unit);

const currentIngredient = computed(() => {
    if(props.ingredient.alternative && useAlternativeIngredient.value) {
        currentUnit.value = props.ingredient.alternative.unit;
        return props.ingredient.alternative;
    } else {
        currentUnit.value = props.ingredient.unit;
        return props.ingredient;
    }
});

const isVolumetric = computed(() => {
    return (props.convertVolume().possibilities() as string[]).includes(currentIngredient.value.unit);
});

const currentConvert = computed(() => {
    return isVolumetric.value
        ? props.convertVolume
        : props.convertMass;
});

const unitOptions = computed(() => {
    const convert = currentConvert.value;
    return convert().list().map((unit) => {
        return {
            value: unit.abbr,
            text: unit.plural
        }
    });
});

function getConvertedUnit(baseValue: number): string {
    return parseFloat(currentConvert.value(baseValue).from(currentIngredient.value.unit).to(currentUnit.value).toFixed(2)) + currentUnit.value;
}

const adjustedAmount = computed(() => {
    return currentIngredient.value.amount * props.factor / props.serves;
});

const adjustedApprox = computed(() => {
    return currentIngredient.value.approx
        ? currentIngredient.value.approx * props.factor / props.serves
        : undefined;
});

const labelText = computed(() => {
    var unitLabel: string;
    if(currentIngredient.value.unit.length > 0) {
        if(adjustedApprox.value) {
            unitLabel = Math.round(adjustedAmount.value).toString() + ` (approx. ${getConvertedUnit(adjustedApprox.value)})`;
        } else {
            unitLabel = getConvertedUnit(adjustedAmount.value);
        }
    } else {
        unitLabel = adjustedAmount.value.toString();
    }
    return `${currentIngredient.value.name}, ${unitLabel}`;
});
</script>