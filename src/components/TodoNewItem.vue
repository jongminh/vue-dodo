<template>
  <v-form ref="newItemForm" v-model="valid">
    <v-text-field
      v-model="todoItem.title"
      :rules="titleRules"
      label="Title"
      required
    ></v-text-field>
    <v-text-field
      v-model="todoItem.content"
      :rules="contentRules"
      label="Content"
      required
    ></v-text-field>
    <v-btn
      :disabled="!valid"
      @click="submit"
    >
      Submit
    </v-btn>
    <v-btn @click="clear">Clear</v-btn>
  </v-form>
</template>

<script>
export default {
  name: "TodoNewItem",
  data: () => ({
    valid: false,
    titleRules: [v => !!v || "Title is required"],
    contentRules: [v => !!v || "Content is required"],
    todoItem: {
      title: "",
      content: ""
    }
  }),
  methods: {
    submit() {
      if (this.$refs.newItemForm.validate()) {
        this.$emit("new-todo-item", this.todoItem);
        this.clear();
      }
    },
    clear() {
      this.$refs.newItemForm.reset();
    }
  }
};
</script>

<style scoped>
</style>