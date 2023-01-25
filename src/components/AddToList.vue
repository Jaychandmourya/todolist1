<template>
  <div class="container" border="1px">
    <div col-md-6>
      <h1>Add To List</h1>
      <div class="col-auto">
        <p style="float: right">
          <router-link to="/">Back To Home</router-link>
        </p>
      </div>
      <div class="row">
        <div class="col-sm-12">
          <label for="exampleFormControlTextarea1" class="form-label"
            >Enter Task</label
          >
          <textarea
            v-model="addtotask"
            class="form-control"
            id="exampleFormControlTextarea1"
            rows="3"
          ></textarea>
        </div>
        <div class="col-sm-12">
          <label for="exampleFormControlTextarea1" class="form-label"
            >Select Priority</label
          >
          <select-priority
            v-bind:selectname="selectpro"
            @selectchangevalue="selectvalue($event)"
          ></select-priority>
        </div>
        <div class="col-auto pt-4">
          <button
            type="submit"
            class="btn btn-primary mb-3"
            @click.prevent="addItem"
          >
            Submit Task
          </button>
        </div>
      </div>
      <div v-if="submitted">
        <p>Data Submitted</p>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import SelectPriority from "./SelectPriority.vue";
export default {
  components: {
    "select-priority": SelectPriority,
  },
  data() {
    return {
      todotask: {
        addtotask: "",
        addpriority: null,
        status: "undone",
        selectedvalue: "",
      },
      todata: [],
      // selectpro: ["high", "medium", "low"],
      selectpro: [{ option: "high" }, { option: "medium" }, { option: "low" }],
      submitted: false,
    };
  },
  methods: {
    async addItem() {
      // console.log(this.selectedvalue);
      try {
        const res = await axios.post(`http://localhost:3000/todos`, {
          taskName: this.addtotask,
          addpriority: this.selectedvalue,
          status: false,
        });
        console.log(res.data);
        this.$router.push("/");
        alert("User Added!");
      } catch (e) {
        console.log(e);
      }
    },
    selectvalue(event) {
      this.selectedvalue = event;
    },
  },
};
</script>
