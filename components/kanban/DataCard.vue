<template>
  <div>
    <v-card class="mx-auto" max-width="400" outlined>
      <div>
        <v-list-item>
          <v-list-item-content>
            <div class="header">
              <div>{{ task.title }}</div>
              <div>
                <v-menu bottom left>
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn dark icon v-bind="attrs" v-on="on">
                      <v-icon color="black" style="font-size: 18px"
                        >mdi-dots-vertical</v-icon
                      >
                    </v-btn>
                  </template>
                  <v-list style="cursor: pointer">
                    <v-list-item @click="$emit('edit')">
                      <v-list-item-title>Edit</v-list-item-title>
                    </v-list-item>
                    <v-list-item @click="$emit('delete')">
                      <v-list-item-title>Delete</v-list-item-title>
                    </v-list-item>
                    <v-list-item @click="addParticipants()">
                      <v-list-item-title>
                        Assign Participants</v-list-item-title
                      >
                    </v-list-item>
                  </v-list>
                </v-menu>
              </div>
            </div>
            <v-list-item-subtitle>{{ task.description }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <div>
          <div v-for="member in members" :key="member.id" class="members">
            <span>{{ member }}</span>
          </div>
        </div>
      </div>
    </v-card>
    <assign-participants
      :item="item"
      @add="addMember(item)"
      ref="participant"
    ></assign-participants>
  </div>
</template>
<script>
import AssignParticipants from "./AssignParticipants.vue";
export default {
  data() {
    return {
      item: "",
      members: [],
    };
  },
  components: {
    AssignParticipants,
  },
  props: {
    task: {
      type: Object,
      default: () => ({}),
    },
  },
  methods: {
    addParticipants() {
      this.$refs.participant.open();
    },
    addMember(item) {
      // this.members.push(item);
      // console.log("please add member");
    },
  },
};
</script>
<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.members {
  width: 50px;
  height: 50px;
  background-color: #c3b2b2;
  border-radius: 50%;
  color: black;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin: 5px 5px;
}
</style>
