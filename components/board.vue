<template>
  <div
    class="board"
    @dragenter.prevent
    @dragover.prevent
    @drop="onDrop($event, data.title)"
  >
    <div class="board_head">
      <input
        class="board_head_input"
        v-model="boardTitle"
        type="text"
        @change="changeTitle(data.boardId, $event)"
        placeholder="Title"
      />
      <span>{{ data.items.length }}</span>
    </div>
    <div class="board_bottom">
      <Card
        v-for="item,index in data.items"
        :key="item.id"
        :item="item"
        :cover="index==0"
        :board="data.title"
      />
    </div>
    <div class="board_new">
      <input
        type="text"
        v-model="task"
        placeholder="New Item"
        autocomplete="true"
        @keyup.enter="newTaskSubmit()"
      />
    </div>
  </div>
</template>

<script>
import Card from "../components/card.vue";

export default {
  props: ["data", "switch", "newTask", "changeTitle"],
  data: function() {
    return {
      task: "",
      boardTitle: this.data.title
    };
  },
  components: {
    Card
  },
  methods: {
    onDrop: function(event, title) {
      const itemId = event.dataTransfer.getData("id");
      const fromBoard = event.dataTransfer.getData("board");
      // console.log("item dropped in ",this.data.title,item)
      this.switch(itemId, fromBoard, title);
    },
    newTaskSubmit: function() {
      const newItem = {
        id: parseInt(Math.random() * 1000),
        title: this.task,
        board: this.data.title
      };
      this.task = "";
      this.newTask(newItem);
    }
  }
};
</script>

<style>
.board {
  min-height: 15rem;
  min-width: 17rem;
  max-width: 17rem;
  background-color: #ebecf0;
  display: flex;
  flex-direction: column;
  padding: 0.5rem 0rem;
  border-radius: 2px;
  margin: 1rem 1rem;
  padding-bottom: 1rem;
}
.board_head {
  padding: 0px 0.5rem;
  margin-bottom: 1rem;
  display: flex;
  width: 100%;
  align-items: center;
}

.board_head span {
  width: 10%;
  background-color: #f3e260;
  border-radius: 1rem;

  padding: 0.2rem 1rem;
  margin-left: 0.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.board_head input {
  width: 90%;
  color: rgb(23, 43, 77);
  font-size: 1.25rem;
  font-weight: 600;
  background-color: inherit;
  border: 1px solid #ebecf0;
  transition: all ease 0.2s;
  font-family: inherit;
}
.board_head input:hover {
  cursor: pointer;
}
.board_head input:focus,
.board_head input:hover {
  /* border:1px solid rgb(23, 43, 77); */
  outline: none;
}
.board_new input {
  padding: 0rem 0.5rem;
  height: 2rem;
  width: 16rem;
  background-color: #fff;
  color: #111;
  margin: 0px auto;
  margin-bottom: 0.5rem;
  display: flex;
  justify-content: left;
  align-items: center;
  border-radius: 2px;
  border: 0px;
}
.board_new input:focus {
  outline: none;
}
</style>
