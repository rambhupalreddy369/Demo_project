<template>
  <div>
    <v-dialog class="container" persistent v-model="dialog" max-width="350">
      <v-card>
        <v-toolbar max-height="48" flat dense>
          <v-toolbar-title> Add List </v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
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
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="onSubmit()">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      dialog: false,
      editList: false,
      form: {
        title: "",
      },
      nameRules: [(v) => !!v || "Name is required"],
    };
  },
  methods: {
    openColumn() {
      this.dialog = true;
    },
    openEditColumn(item) {
      this.dialog = true;
      this.editList = true;
      this.form.title = item?.title || "";
    },
    resetForm() {
      this.form = {
        title: "",
      };
    },
    onSubmit() {
      if (this.form.title.length > 0) {
        if (this.editList) {
          this.$emit("editColumn", this.form);
        } else {
          this.$emit("onAdd", this.form);
        }
        this.dialog = false;
        this.editList = false;
        this.resetForm();
      } else {
        alert("Please Enter Title");
      }
    },
  },
};
</script>
