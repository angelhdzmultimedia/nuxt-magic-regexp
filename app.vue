<script lang="ts" setup>
import { createRegExp, exactly, maybe, oneOrMore, digit, char, word, wordChar, global, multiline} from 'magic-regexp'

const text = 'My name is {name} and I\'m {age} years old!';
const regExp = createRegExp(

    exactly('{'),
    oneOrMore(wordChar).grouped(),
  exactly('}'),
 
  [global, multiline]
)
const data = ref<{name: string, value: string}[]>([
  {name: 'name', value: 'Angel'}
])


const replacedText = text.replaceAll(regExp, (match, key: string) => {
  const _data: any = Object.fromEntries(data.value.map(({name, value}) => [name, value]))
  return _data[key] || `{${key}}`
})//

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

console.log(replacedText)
</script>

<template>
  <div class="column">
   <q-table :rows="data" :columns="columns"/>
  </div>
</template>
