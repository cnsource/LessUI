<script setup>

import InputSelect from "../../InputSelect.vue";
import {ref} from "vue";

const val = ref('')
const options = ref([
  {body: 'Scoped Slots Guide', username: 'Evan You', likes: 20},
  {body: 'Vue Tutorial', username: 'Natalia Tepluhina', likes: 10}
])

function onChange(value) {
  console.log(value)
}

const buttonPlaygroundCode = ` <input-select
          :options="options"
          v-model:value="val"
          bind-prop="body"
          @change="onChange">
        <template v-slot="{body, username, links}">
          {{ body }} - {{ username }}
        </template>
      </input-select>`;
</script>

# Button component

## Description

Very simple button component

## Props

<Props :of="Button"></Props>

## Example

<Playground 
  :code="buttonPlaygroundCode"
  :components="{ Button }">
</Playground>
