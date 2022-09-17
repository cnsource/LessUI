<script setup>
import { onActivated, ref, toRaw, watch } from 'vue';

const showOption = ref(false);
const inputValue = ref('');
const isOptionValue = ref(false);
const chooseOption = ref(null);
const inputElement = ref();

const props = defineProps({
  value: {
    type: [Object, Number, String],
    default: '',
  },
  disable: {
    type: Boolean,
    default: false,
  },
  rounded: {
    type: Boolean,
    default: false,
  },
  roundedFull: {
    type: Boolean,
    default: false,
  },
  options: {
    type: Array,
    default: [],
  },
  bindProp: {
    type: String,
    default: undefined,
  },
});
const emits = defineEmits(['update:value', 'change']);

watch(props, (val, oldValue) => {
  inputValue.value = val.value;
});

watch(inputValue, (val, oldValue) => {
  emits('update:value', inputValue.value);
});

function onFocus() {
  showOption.value = true;
}

function onBlur() {
  let isChoose = false;
  if (isOptionValue.value) {
    // 用户选择的选项
    isChoose = true;
  }
  emits('update:value', inputValue.value);
  emits('change', {
    value: inputValue.value,
    option: toRaw(chooseOption.value),
    isChoose,
  });
  isOptionValue.value = false;
  showOption.value = false;
  chooseOption.value = null;
}

function onChooseOption(option) {
  chooseOption.value = option;
  isOptionValue.value = true;
  inputValue.value = option[props.bindProp] ?? option;
}

function onEnter() {
  inputElement.value.blur();
}

onActivated(() => {
  inputValue.value = props.value;
});
</script>

<template>
  <div>
    <div class="relative w-full h-full">
      <div
        v-if="disable"
        class="bg-gray-200/30 bg-opacity-25 cursor-not-allowed absolute w-full h-full"
      ></div>
      <div class="flex items-center border">
        <input
          type="text"
          ref="inputElement"
          @focus="onFocus"
          @blur="onBlur"
          v-model="inputValue"
          @keydown.enter="onEnter"
          class="input-style p-1.5 focus:outline-none w-full h-full peer"
          :class="{ rounded: rounded, 'rounded-full px-2': roundedFull }"
        />
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="text-transparent hidden w-6 h-6 bg-transparent peer-focus:block peer-focus:text-gray-400 peer-focus:transition peer-focus:transform"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M19.5 8.25l-7.5 7.5-7.5-7.5"
          />
        </svg>
      </div>
      <ul
        class="option-box-style absolute w-full"
        :class="{ hidden: !showOption }"
      >
        <li
          v-for="option in options"
          @mousedown.stop="onChooseOption(option)"
          class="option-style p-1 w-full hover:bg-gray-200 select-none"
        >
          <slot v-bind="option">
            {{ option[bindProp] ?? option }}
          </slot>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped></style>
