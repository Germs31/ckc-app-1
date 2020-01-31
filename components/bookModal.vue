<template>
  <v-dialog max-width="500px" v-model="open">
    <v-card class="p-5">
      <v-card-title>Add Books</v-card-title>
      <v-container>
        <v-form ref="form" v-model="valid">
          <v-select v-model="category" :items="categories" label="Select A Category" :rules="catRules" required></v-select>
          <v-text-field v-model="title" label="Enter Book Title" :rules="titleRules" required></v-text-field>
          <v-text-field v-model="author" label="Enter Book Author" :rules="authorRules" required></v-text-field>
          <v-textarea v-model="description" label="Enter Book Description" :rules="desRules" required></v-textarea>
        </v-form>
      </v-container>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn @click.stop="saveBook" color="green">Add</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import { eventBus } from "@/eventBus";
export default {
  data() {
    return {
      open: false,
      category: "",
      title: "",
      author: "",
      description: "",
      categories: ["Recently read books", "Favourite books", "Best of the best"],
      catRules: [
        v => !!v || 'Catagory is required',
      ],
      titleRules: [
        v => !!v || 'Title is required',
      ],
      authorRules: [
        v => !!v || 'Author is required',
      ],
      desRules: [
        v => !!v || 'Description is required',
      ]
    };
  },
  created() {
    eventBus.$on("open-add-book-modal", data => {
      if (data) {
        this.category = data.category;
        this.title = data.title;
        this.author = data.author;
        this.description = data.description;
      }
      this.open = true;
    });
  },
  methods: {
    saveBook() {
      if (this.$refs.form.validate()) {
        this.snackbar = true
      }
      let cardData = {
        title: this.title,
        author: this.author,
        description: this.description,
        category: this.category
      };
      if( cardData.title == '' || 
          cardData.author == '' || 
          cardData.description == '' || 
          cardData.category == '' || 
          cardData == ''){
        return this.open=true;
      }
      eventBus.$emit("save-book", cardData);
      this.open = false;
      this.$refs.form.reset();
    }
  }
};
</script>