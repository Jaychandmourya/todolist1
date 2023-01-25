<template>
  <div>
    <v-select
      v-model="selected"
      :options="selectname"
      :reduce="(option) => option.option"
      label="option"
      @input="inputselect"
    />
    <li v-for="data in selectname" :key="data.option">{{ data }}</li>
  </div>
</template>
<script>
export default {
  props: ["selectname"],
  data() {
    return {
      selected: "",
    };
  },
  methods: {
    inputselect() {
      this.$emit("selectchangevalue", this.selected); // event emitted to parent component with selected Date
    },
  },
};
</script>
