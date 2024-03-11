<script lang="ts" setup>
import { createRegExp, exactly, maybe, oneOrMore, digit, char, word, wordChar, global, multiline} from 'magic-regexp'

const text = 'My name is {name} and I\'m {age} years old!';
const regExp = createRegExp(

    exactly('{'),
    oneOrMore(wordChar).grouped(),
  exactly('}'),
 
  [global, multiline]
)
const data = {
  name: 'Angel',
  age: 38
}

const replacedText = text.replaceAll(regExp, (match, key) => {
  return data[key] || `{${key}}`
})//

console.log(replacedText)
</script>

<template>
  <div>
   <h1>Index</h1>
  </div>
</template>
