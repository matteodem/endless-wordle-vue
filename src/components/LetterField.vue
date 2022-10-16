<template>
  <div class="letter-field" :class="type">&nbsp;{{ value }}&nbsp;</div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
    },
    type: {
      type: String,
    },
  },
};
</script>
<style>
.letter-field {
  height: 24px;
  width: 24px;
  border: 1px solid black;
  font-size: 1rem;
  font-weight: bold;
  text-transform: uppercase;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin: 2px 0;
}

.letter-field.no-match {
  border: 1px solid gray;
  background-color: darkgray;
  opacity: 0.8;
  color: gray;
}

.letter-field.match {
  border: 1px solid orange;
  background-color: #ff5733;
  color: orange;
}

.letter-field.perfect-match {
  border: 1px solid green;
  background-color: lightgreen;
  color: green;
}
</style>
