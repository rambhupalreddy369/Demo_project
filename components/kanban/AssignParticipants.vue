<template>
  <v-dialog persistent v-model="dialog" max-width="300">
    <v-card>
      <v-toolbar max-height="40" flat dense>
        <v-toolbar-title class="" style="font-size: 16px"
          >Add Members</v-toolbar-title
        >
        <v-spacer></v-spacer>
        <v-toolbar-items>
          <v-btn icon @click="onClose">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-toolbar-items>
      </v-toolbar>
      <v-divider></v-divider>
      <div>
        <div style="margin: 0 10px" class="mt-2">
          <v-text-field
            max-width="260"
            solo
            single-line
            label="Search Participant"
            v-model="participant"
          >
          </v-text-field>
        </div>
        <div
          v-for="member in searchItem"
          :key="member.name"
          style="margin-left: 10px"
        >
          <span
            style="cursor: pointer; padding: 5px"
            @click="addMembers"
            class="member"
            >{{ member.name }}</span
          >
        </div>
        <!-- <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn @click="addMembers">Add Member</v-btn>
        </v-card-actions> -->
      </div>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      participant: "",
      members: [{ name: "ram" }, { name: "Bhargav" }, { name: "Himavan" }],
    };
  },
  computed: {
    searchItem() {
      console.log(this.members);
      return this.members?.filter((member) =>
        member.name.includes(this.participant)
      );
    },
  },
  methods: {
    open() {
      this.dialog = true;
    },
    onClose() {
      this.dialog = false;
    },
    // onClickUser(member, index) {
    //   this.members.splice(index, 1)
    //   this.selected.push(member)
    // },
    addMembers(item) {
      this.members = item;
      this.$emit("add", item);
    },
  },
};
</script>

<style scoped>
.member {
  padding: 10px;
  font-size: 20px;
  text-transform: capitalize;
}
</style>
