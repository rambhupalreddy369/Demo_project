<template>
  <div>
    <div class="d-flex justify-content-around">
      <div class="d-flex col-sm tasks bg-transperant">
        <div>
          <draggable group="columns" v-model="columns" tag="v-layout">
            <div
              v-for="(column, columnIndex) in columns"
              :key="column.title"
              :item="column"
              class="mr-2 task"
              style="max-width: 320px; min-width: 320px"
            >
              <div class="list-header pa-2">
                <div class="header">
                  <span class="title">{{ column.title }}</span>
                  <v-menu bottom left>
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn dark icon v-bind="attrs" v-on="on">
                        <v-icon color="black" style="font-size: 18px"
                          >mdi-dots-vertical</v-icon
                        >
                      </v-btn>
                    </template>

                    <v-list>
                      <v-list-item @click="openEditList(columnIndex, column)">
                        <v-list-item-title>Edit </v-list-item-title>
                      </v-list-item>
                      <v-list-item @click="deleteList(columnIndex)">
                        <v-list-item-title>Delete</v-list-item-title>
                      </v-list-item>
                    </v-list>
                  </v-menu>
                </div>
                <v-btn style="margin: 4px" @click="openAddCard(columnIndex)">
                  <v-icon>mdi-plus</v-icon> Add Card</v-btn
                >
              </div>
              <div class="list-items">
                <draggable
                  :list="column.tasks"
                  @change="onListChange"
                  ghost-class="ghost-card"
                  :animation="200"
                  group="tasks"
                  @end="onDraggingEnd"
                  v-model="tasks"
                >
                  <!-- @edit="(item) => updateCard(taskIndex, columnIndex, item)" -->
                  <data-card
                    v-for="(task, taskIndex) in column.tasks"
                    :key="taskIndex"
                    :task="task"
                    @edit="openEditCard(taskIndex, columnIndex, task)"
                    @delete="deleteCard(taskIndex, columnIndex)"
                    class="mb-3 cursor-move"
                  >
                  </data-card>
                </draggable>
              </div>
            </div>
          </draggable>
        </div>
        <div class="addlist">
          <v-btn style="margin: 4px" @click="openList">
            <v-icon>mdi-plus</v-icon> Add Another List</v-btn
          >
        </div>
      </div>
    </div>
    <add-card
      :editItem="selectedItem"
      :editItemIndex="selectedEditIndex"
      :editColumnIndex="selectedAddIndex"
      @add="onAdd"
      @edit="updateCard"
      ref="kanban"
    ></add-card>
    <add-list-card
      @onAdd="addList"
      @editColumn="editListName"
      ref="column"
    ></add-list-card>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import DataCard from "@/components/kanban/DataCard.vue";
import AddCard from "@/components/kanban/AddCard.vue";
import AddListCard from "@/components/kanban/AddListCard.vue";
export default {
  layout: "plain",
  components: {
    DataCard,
    draggable,
    AddCard,
    AddListCard,
  },
  data() {
    return {
      selectedAddIndex: null,
      selectedEditIndex: null,
      selectedItem: null,
      selectedList: null,
      dragging: false,
      draggingIndex: -1,
      listChanged: false,
      dragged: false,
      task: "",
      item: "",
      columns: [
        {
          title: "Unsolved",
          tasks: [
            {
              title: "Task1",
              description: "this is the description of the canban task",
            },
            {
              title: "Task2",

              description: "this is the description of the canban task",
            },
          ],
        },
        {
          title: "Solving",
          tasks: [
            {
              id: 3,
              title: "Task1",
              description: "this is the description of the canban task",
            },
            {
              id: 4,
              title: "Task2",
              description: "this is the description of the canban task",
            },
          ],
        },
        {
          title: "Solved",
          tasks: [
            {
              id: 5,
              title: "Task1",
              description: "this is the description of the canban task",
            },
            {
              id: 6,
              title: "Task2",
              description: "this is the description of the canban task",
            },
          ],
        },
      ],
    };
  },
  methods: {
    openList() {
      this.$refs.column.openColumn();
    },
    addList(item) {
      const column = {
        title: item.title,
        tasks: [],
      };
      this.columns.push(column);
    },
    deleteList(columnIndex) {
      console.log("delete column");
      this.columns.splice(columnIndex, 1);
    },
    openEditList(columnIndex, column) {
      this.selectedAddIndex = columnIndex;
      this.selectedList = this.columns[columnIndex];
      this.$refs.column.openEditColumn(column);
    },
    editListName(item) {
      const column = this.columns[this.selectedAddIndex];
      column.title = item.title;
      this.columns.splice(this.selectedAddIndex, 1, column);
    },
    onListChange() {
      this.listChanged = true;
    },
    onDraggingEnd(e) {
      this.dragged = true;
    },
    onAdd(item) {
      this.columns[this.selectedAddIndex].tasks.unshift(item);
      console.log(item);
    },
    openAddCard(index) {
      this.selectedAddIndex = index;
      this.$refs.kanban.open();
    },
    openEditCard(taskIndex, columnIndex, task) {
      this.selectedAddIndex = columnIndex;
      this.selectedEditIndex = taskIndex;
      this.selectedItem = this.columns[columnIndex].tasks[taskIndex];
      this.$refs.kanban.openEditForm(task);
    },
    updateCard(item) {
      this.columns[this.selectedAddIndex].tasks.splice(
        this.selectedEditIndex,
        1,
        item
      );
      // this.columns[this.selectedAddIndex].tasks[this.selectedEditIndex] = item
    },
    deleteCard(taskIndex, columnIndex) {
      this.columns[columnIndex].tasks.splice(taskIndex, 1);
    },
  },
};
</script>

<style scoped>
.sortable-chosen {
  background: #c8ebfb;
}
@media only screen and (max-width: 959px) {
  .container {
    padding: 0px;
  }
}
.ghost-card {
  opacity: 0.5;
  background: #c8ebfb;
  border: 1px solid #4299e1;
}
.title {
  text-align: center;
}
.tasks {
  overflow-x: scroll;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 5px;
}
.task {
  background-color: #c8ebfb;
}

.list-items {
  max-height: 600px;
  overflow: auto;
  padding: 0px 10px;
}
/* .addlist {
  display: inline;
} */
</style>
