<template>
  <div class="keyboard">
    <div
      v-for="key in keyboardKeys"
      :class="[
        {
          'enter-key': key.value === 'enter',
          'delete-key': key.value === 'delete',
        },
        getHint(key),
      ]"
      class="key"
      @click="guess(key)"
    >
      <div v-if="key.value.length === 1">
        {{ key.value.toUpperCase() }}
      </div>
      <div v-if="key.value === 'enter'" class="enter">ENTER</div>
      <div v-if="key.value === 'delete'" class="delete">DELETE</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    hints: {
      type: Array,
    },
  },
  data() {
    return {
      keyboardKeys: [
        { value: 'q' },
        { value: 'w' },
        { value: 'e' },
        { value: 'r' },
        { value: 't' },
        { value: 'z' },
        { value: 'u' },
        { value: 'i' },
        { value: 'o' },
        { value: 'p' },
        { value: 'a' },
        { value: 's' },
        { value: 'd' },
        { value: 'f' },
        { value: 'g' },
        { value: 'h' },
        { value: 'j' },
        { value: 'k' },
        { value: 'l' },
        { value: 'enter' },
        { value: 'y' },
        { value: 'x' },
        { value: 'c' },
        { value: 'v' },
        { value: 'b' },
        { value: 'n' },
        { value: 'm' },
        { value: 'delete' },
      ],
    };
  },
  methods: {
    guess(key) {
      this.$emit('guess', key.value);
    },
    getHint(key) {
      const { value } = key;

      const matched = this.hints.filter((hint) => hint.value === value);

      if (matched.length === 0) {
        return;
      }

      const newestMatch = matched[matched.length - 1];

      if (newestMatch) {
        console.log(newestMatch);
        return newestMatch.type;
      }
    },
  },
};
</script>
<style>
.keyboard {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;

  width: 360px;
  margin: 30px auto 0 auto;
}

.keyboard .key {
  width: 28px;
  height: 24px;
  margin: 3px;
  border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
  cursor: pointer;
}

.keyboard .enter-key,
.keyboard .delete-key {
  width: 46px;
}

.keyboard .enter,
.keyboard .delete {
  width: 40px;
  font-size: 0.6rem;
}

.keyboard .key.no-match {
  border: 1px solid gray;
  background-color: darkgray;
  opacity: 0.8;
  color: gray;
}

.keyboard .key.match {
  border: 1px solid orange;
  background-color: #ff5733;
  color: orange;
}

.keyboard .key.perfect-match {
  border: 1px solid green;
  background-color: lightgreen;
  color: green;
}
</style>
