<template>
  <div class="word-field">
    <div v-for="letter in guess" class="letter">
      <LetterField :value="letter.value" :type="letter.type" />
    </div>
  </div>
</template>

<script>
import LetterField from './LetterField';

export default {
  components: {
    LetterField,
  },
  props: {
    guess: {
      type: Array,
    },
  },
};
</script>
<style>
.word-field > .letter {
  display: inline-block;
  margin: 0 2px;
}
</style>
