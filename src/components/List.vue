<template>
  <p class="text-center text-h5 font-weight-medium">List Todo</p>
  <v-text-field
  id="search"
    class="mt-1"
    :loading="loading"
    density="compact"
    variant="outlined"
    clearable
    label="Cari List Todo"
    v-model="search"
    append-inner-icon="mdi-magnify"
    hide-details
    @click:append-inner="onClick"
  ></v-text-field>

  <v-card
    v-for="(todo) in todosFilter"
    :key="todo.id"
    variant="outlined"
    class="my-2"
  >
    <v-card-text
      class="d-flex flex-wrap justify-space-between py-2 ga-2 align-center"
    >
      <span v-if="todo.isDone">
        <del
          ><p class="text-h6">{{ todo.activity }}</p></del
        >
      </span>
      <span v-else
        ><p class="text-h6">{{ todo.activity }}</p></span
      >

      <div class="d-flex align-center ms-auto justify-end ga-2">
        <v-btn
          v-if="todo.isDone"
          @click="doneTODO(todo.id)"
          density="comfortable"
          color="amber"
          >Ulangi</v-btn
        >
        <v-btn
          v-else
          density="comfortable"
          @click="doneTODO(todo.id)"
          color="green"
          >Done</v-btn
        >
        <v-btn
          @click="deleteTODO(todo.id)"
          density="compact"
          color="red"
          icon="mdi-delete"
        ></v-btn>
      </div>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  emits: ['deleteTodo', 'doneTodo'],
  data() {
    return {
      loading: false,
      search: "",
      todosFilter: [],
    };
  },
  watch: {
    todos() {
      this.todosFilter = this.todos
    }
  },
  methods: {
    onClick(e) {
      e.stopPropagation();
      document.querySelector("#search").blur()
      this.loading = "blue";
      if(this.search){
        this.todosFilter = this.todos.filter((item) => {
          return item.activity.toLowerCase().includes(this.search.toLowerCase());
        })
      } else {
        this.todosFilter = this.todos
      }
      setTimeout(()=>{
        this.loading = false
      }, 1000)
    },
    deleteTODO(id) {
      this.$emit("deleteTodo", id);
    },
    doneTODO(id) {
      this.$emit("doneTodo", id);
    },
  },
};
</script>

<style></style>
