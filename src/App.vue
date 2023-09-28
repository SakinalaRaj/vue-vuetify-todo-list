<template>
  <h1>Welcome to Vue Todo list</h1>
  <div>
    <div>
      <v-responsive class="mx-auto" max-width="980">
        <v-text-field
          label="Task"
          v-model="name"
          placeholder="enter here"
        ></v-text-field>
      </v-responsive>

      <v-btn
        btn
        color="green"
        btn-info
        size="x-large"
        rounded="xl"
        @click="saveHandler"
      >
        Save Task
      </v-btn>
    </div>
  </div>

  <v-container v-for="(item, index) in dataArray" :key="index">
    <v-row no-gutters>
      <v-col cols="8" class="text-left">
        <v-sheet class="pt-5"> {{ index + 1 }}. {{ item }} </v-sheet>
      </v-col>

      <v-col cols="4">
        <v-sheet class="pa-2 ma-2">
          <v-btn
            class="ma-6"
            color="blue"
            size="x-large"
            variant="outlined"
            @click="editHandler(index)"
          >
            Edit
          </v-btn>
          <v-btn size="x-large" color="red" @click="deleteHandler(index)">
            Delete
          </v-btn>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
  {{ console.log("this.dataArray", this.dataArray) }}
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},

  data() {
    return {
      name: "",
      dataArray: [],
      editOperation: false,
      editIndex: null,
    };
  },
  mounted() {
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then(({ data }) => (this.dataArray = data.map((item) => item.name)))
      .catch((err) => console.log("err", err));
  },
  methods: {
    saveHandler() {
      if (!this.editOperation) {
        this.dataArray.push(this.name);
      } else {
        this.dataArray.splice(this.editIndex, 1, this.name);
      }

      this.name = "";
      // console.log("this.dataArray", this.dataArray);
      this.editOperation = false;
    },
    deleteHandler(index) {
      this.dataArray.splice(index, 1);
    },
    editHandler(index) {
      this.name = this.dataArray[index];
      this.editOperation = true;
      this.editIndex = index;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
