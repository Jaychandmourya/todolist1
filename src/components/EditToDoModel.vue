<template>
  <div class="container">
    <!-- <button
      type="button"
      class="btn btn-primary"
      data-bs-toggle="modal"
      data-bs-target="#exampleModal"
    >
      Launch demo modal
    </button> -->

    <!-- Modal -->
    <div
      v-if="OpenClose"
      class="modal fade show"
      style="display: block"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Edit To Do List</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              @click="CloseFun"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <div class="col-sm-12">
              <label for="exampleFormControlTextarea1" class="form-label"
                >Enter Task</label
              >
              <textarea
                v-model="editdata.taskName"
                class="form-control"
                id="exampleFormControlTextarea1"
                rows="3"
              ></textarea>
            </div>
            <div class="form-group">
              <label for="exampleFormControlSelect1">Select Priority</label>
              <select
                class="form-control"
                id="exampleFormControlSelect1"
                v-model="editdata.addpriority"
              >
                <option>Select Priority</option>
                <option
                  selected
                  v-for="selectdata in selectpro"
                  :key="selectdata"
                >
                  {{ selectdata }}
                </option>
              </select>
            </div>
            <!-- <li v-for="data in editlistdata">{{ data }}</li> -->
          </div>
          <div class="modal-footer">
            <button
              type="button"
              data-bs-dismiss="modal"
              @click="CloseFun"
              :class="'btn btn-' + variant"
            >
              Close
            </button>

            <button
              type="button"
              @click="OpenCloseFun(editdata)"
              class="btn btn-primary"
            >
              Save changes
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "EditToDoModel",
  props: {
    visible: Boolean,
    variant: String,
    editlistdata: Array,
  },
  data() {
    return {
      OpenClose: this.visible,
      selectpro: ["high", "medium", "low"],
      editdata: this.editlistdata,
    };
  },
  methods: {
    async OpenCloseFun(editdata) {
      console.log(editdata);
      try {
        const user = await axios.put(
          "http://localhost:3000/todos/" + editdata.id,
          {
            taskName: this.editdata.taskName,
            addpriority: this.editdata.addpriority,
            status: editdata.status,
          }
        );

        console.log(user.data);
        this.OpenClose = !this.OpenClose;
        this.$emit("close-modal-event");
        // this.$router.push("/");
        // alert("User updated!");
      } catch (e) {
        console.log(e);
      }
      //   this.OpenClose = !this.OpenClose;
      //   this.$emit("close-modal-event");
    },
    CloseFun() {
      this.OpenClose = !this.OpenClose;
      this.$emit("close-modal-event");
    },
  },
  watch: {
    visible: function (newVal) {
      this.OpenClose = newVal;
      //   this.OpenClose = oldVal;
    },
  },
};
</script>
