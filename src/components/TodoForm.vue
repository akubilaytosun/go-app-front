<template>
  <v-container>
    <v-card color="#E8EAF6" class="ma-2">
      <v-row>
        <v-col>
          <v-card-text class="title">To Do</v-card-text>
        </v-col>
      </v-row>
      <v-card-text>
        <v-form ref="entryForm" @submit.prevent="createTodo()">
          <v-row style="justify-content: center">
            <v-col cols="4">
              <v-text-field
                :rules="[rules.required]"
                style="height: 50px"
                label="Başlık"
                outlined
                solo
                class="title"
                v-model="formInput.title"
              ></v-text-field>
            </v-col>
            <v-col cols="2">
              <v-btn
                depressed
                color="success"
                @click="createTodo()"
                style="height: 50px"
                >Kaydet</v-btn
              >
            </v-col>
          </v-row>
        </v-form>

        <v-row style="justify-content: center">
          <v-col cols="5">
            <v-alert dense text type="success" v-if="alertControlSuccess">
              Başarılı bir şekilde kaydedildi.
            </v-alert>
            <v-alert dense text type="error" v-if="alertControlError">
              Lütfen Başlık Alanını Doldurunuz.
            </v-alert>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
    <v-card color="#E8EAF6" class="ma-2 ">
      <h1 class="title">To Do List</h1>
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Ara"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="todos"
        :search="search"
      ></v-data-table>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    formInput: {
      title: "",
    },
    alertControlSuccess: false,
    alertControlError: false,
    rules: {
      required: (value) => !!value || "Zorunlu Alan",
    },
    search: "",
    todos: [],
    headers: [
      {
        text: "İd",
        align: "start",
        value: "id",
      },
      {
        text: "Başlık",
        value: "title",
      },
    ],
  }),
  mounted() {
    this.getTodos();
  },
  methods: {
    createTodo() {
      if (this.$refs.entryForm.validate()) {
        let body = {
          title: this.formInput.title,
        };
        this.axios
          .post(`http://localhost:8000/todos`, body)
          .then((response) => {
            console.log("response post geldi", response.data);
            this.alertControlSuccess = true;
            this.getTodos();
          })
          .catch((error) => {
            console.log("err", error);
            this.alertControlError = true;
          })
          .then(() => {
            setTimeout(() => {
              this.alertControlSuccess = false;
              this.alertControlError = false;
            }, 2000);
          });
      }
    },
    getTodos() {
      this.axios.get(`http://localhost:8000/todos`).then((response) => {
        console.log("response geldi", response.data);
        this.todos = response.data;
      });
    },
  },
};
</script>

<style></style>
