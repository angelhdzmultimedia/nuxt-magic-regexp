<script lang="ts" setup>
import { createRegExp, exactly, maybe, oneOrMore, digit, char, word, wordChar, global, multiline} from 'magic-regexp'

const regExp = createRegExp(
  exactly('{'),
  oneOrMore(wordChar).grouped(),
  exactly('}'),
  [global, multiline]
)
const variables = ref<{name: string, value: string}[]>([])

const columns = [
  {
    name: 'name',
    key: 'name',
    field: 'name',
    label: 'Name'
  },

  {
    name: 'value',
    key: 'value',
    field: 'value',
    label: 'Value'
  },
]

const form = ref({
  name: '',
  value: '', 
  text: ''
})

function addVariable() {
  variables.value.push({...form.value})
  form.value.name = ''
  form.value.value = ''
}

const text = ref('')

function replaceText() {
  text.value = form.value.text.replaceAll(regExp, (match, key: string) => {
  const _data: any = Object.fromEntries(variables.value.map(({name, value}) => [name, value]))
  return _data[key] || `{${key}}`
})
form.value.text = ''
}

function removeVariables() {
  variables.value = []
}
</script>

<template>
  <div class="column q-pa-md">
   <q-table  row-key="name" :rows="variables" :columns="columns">
    <template #top>
     <div class="row q-gutter-x-md items-center">
      <span class="text-primary text-h6">Variables</span>
      <q-btn flat icon="delete" @click="removeVariables" color="negative" label="Remove Variables"/>
     </div>
    </template>
  </q-table>
   <q-input v-model="form.name" label="Name"/>
   <q-input v-model="form.value" label="Value"/>
   <div class="row">
    <q-btn color="primary" @click="addVariable" label="Add Variable"/>
   </div>
   <div class="column q-pt-md">
    <span class=" text-h6 text-primary">Replace Text</span>
    <q-input v-model="form.text" label="Text"/>
    <q-btn @click="replaceText" color="primary" label="Replace Text"/>
    <span class="text-h6 text-primary">{{ text || '<Empty>' }}</span>
   </div>

  </div>
</template>
