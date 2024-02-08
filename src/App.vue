<template>
  <v-theme-provider theme="light">
    <v-container class="spacing-playground pa-6">
      <v-row>
        <v-col>
          <v-card class="mx-auto py-5 px-3" color="grey-darken-4">
            <v-card-title class="text-h4 font-weight-bold"
              >Simple Todo APP</v-card-title
            >
            <v-card-text class="my-2 pb-0">
              <v-form @submit.prevent="add">
                <v-row no-gutters class="ga-3 my-3">
                  <v-col>
                    <v-text-field
                      clearable
                      density="compact"
                      v-model="todo"
                      :error-messages="error"
                      label="Masukan List Todo"
                      variant="outlined"
                      @change="error = []"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="2">
                    <v-hover>
                      <template v-slot:default="{ isHovering, props }">
                        <v-btn
                          style="height: 40px"
                          type="submit"
                          v-bind="props"
                          :variant="isHovering ? 'elevated' : 'outlined'"
                          color="primary"
                          :loading="loading"
                          block
                          >ADD</v-btn
                        >
                      </template>
                    </v-hover>
                  </v-col>
                </v-row>
              </v-form>
              <v-container fluid>
                <List
                  :todos="todos"
                  @deleteTodo="deleteTodo"
                  @doneTodo="doneTodo"
                />
              </v-container>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-theme-provider>
</template>

<script>
import List from "./components/List.vue";

export default {
  components: {
    List,
  },
  data() {
    return {
      todo: "",
      todos: [],
      loading: false,
      error: [],
    };
  },
  mounted() {
    this.loadTodos();
  },
  computed: {
    totalTODO() {
      return this.todos.length;
    },
  },
  methods: {
    add() {
      this.loading = true;
      if (!this.todo) {
        this.error = ["List Todo Tidak Boleh Kosong"];
        this.loading = false;
        return;
      }
      if (
        this.todos.filter(
          (item) => item.activity.toLowerCase() == this.todo.toLowerCase()
        ).length > 0
      ) {
        this.error = ["List Todo Tidak Boleh Sama"];
        this.loading = false;
        return;
      }
      this.error = [];
      this.todos = [
        // Re-assignment
        {
          id: Date.now(),
          activity: this.todo,
          isDone: false,
        },
        ...this.todos, // Menambahkan elemen yang sudah ada kembali ke dalam array
      ];
      this.todo = "";
      this.saveToLocalStorage();
      setTimeout(() => {
        this.loading = false;
      }, 1000);
    },
    deleteTodo(todoId) {
      this.todos = this.todos.filter((item) => {
        return item.id != todoId;
      });
      this.saveToLocalStorage();
    },
    doneTodo(todoId) {
      this.todos.forEach((item) => {
        if (item.id == todoId) {
          item.isDone = !item.isDone;
        }
      });
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
    loadTodos() {
      const items = localStorage.getItem("todos");
      this.todos = items ? JSON.parse(items) : [];
    },
  },
};
</script>

<style></style>
