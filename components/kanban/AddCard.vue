<template>
  <v-app>
    <div justify="center">
      <v-dialog class="container" persistent v-model="dialog" max-width="350">
        <v-card>
          <v-toolbar max-height="48" flat dense>
            <!-- fix_lang -->
            <v-toolbar-title> Add Problem </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-toolbar-items>
              <!-- <v-btn flat   @click="addStory">Save</v-btn> -->
              <v-btn icon @click="dialog = false">
                <v-icon>mdi-close</v-icon>
              </v-btn>
            </v-toolbar-items>
          </v-toolbar>
          <v-divider></v-divider>
          <v-card-text class="mt-4">
            <v-text-field
              solo
              v-model="form.title"
              label="Title"
              :rules="nameRules"
              type="text"
              required
            ></v-text-field>
            <v-textarea
              solo
              name="description"
              label="Description"
              v-model="form.description"
            ></v-textarea>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="onSumbit">Save</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      editForm: false,
      form: {
        title: "",
        description: " ",
      },
      // image,
      dialog: false,
      nameRules: [(v) => !!v || "Name is required"],
    };
  },
  computed: {
    cardTitle() {
      this.editForm == true ? "Edit Problem" : "Add Problem";
    },
  },
  // created() {
  //   this.form.title = this.editItem?.title || ''
  //   this.form.description = this.editItem?.description || ''
  //   console.log(this.editItem)
  // },
  methods: {
    open() {
      this.dialog = true;
    },
    openEditForm(item) {
      this.dialog = true;
      this.editForm = true;
      this.form.title = item?.title || "";
      this.form.description = item?.description || "";
    },
    resetForm() {
      this.form = {
        title: "",
        description: " ",
      };
    },
    onSumbit() {
      if (this.form.title.length > 5) {
        if (this.editForm) {
          this.$emit("edit", this.form);
        } else {
          this.$emit("add", this.form);
        }
        this.dialog = false;
        this.editForm = false;
        this.resetForm();
      } else {
        alert("Please enter Title");
      }
    },
  },
};
</script>
<style scoped>
.container {
  max-width: 300px;
  background: green;
  z-index: 10;
}
</style>
