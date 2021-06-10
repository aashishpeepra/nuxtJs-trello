<template>
  <div class="container">
    <div class="board-container" v-if="data.length>0">
      <Board
        v-for="board in data"
        :key="board.title"
        :data="board"
        :switch="switchCard"
        :newTask="newTask"
        :changeTitle="changeTitle"
      />
      <div class="new-board">
        <input
          type="text"
          placeholder="New Board"
          autocomplete="true"
          v-model="boardTitle"
          @keyup.enter="addNewBoard"
        />
      </div>
    </div>
  <div v-else>
    <h2>Loading Database... </h2>
  </div>
  </div>
</template>

<script>
import constants from "../helper/constants";
import Board from "../components/board.vue";
import localforage from "localforage";
export default {
  data: () => {
    return {
      boardTitle: "",
      data: []
    };
  },
  beforeCreate() {
    localforage.getItem("data").then(response => {
      if (!response) {
        localforage.setItem("data", constants.json);
        this.data = constants.json;
      } else {
        this.data = response;
      }
    });
  },
  components: {
    Board
  },
  methods: {
    saveData: function() {
      localforage.setItem("data", this.data);
    },

    addNewBoard: function() {
      this.data.push({
        title: this.boardTitle,
        boardId: parseInt(Math.random() * 1000),
        items: []
      });
      this.boardTitle = "";
      this.saveData();
    },
    changeTitle: function(boardId, event) {
      let selectedBoard = this.data.find(board => board.boardId == boardId);
      selectedBoard.title = event.target.value;

      console.log(selectedBoard);
      this.saveData();
    },
    newTask: function(item) {
      console.log(item);
      let selectedBoard = this.data.find(board => board.title == item.board);
      selectedBoard.items.push(item);
      this.saveData();
    },
    switchCard: function(itemId, preBoard, newBoard) {
      console.log(itemId, preBoard, newBoard);
      const selectedBoard = this.data.find(board => board.title == preBoard);
      let position = 0;
      const selectedItem = selectedBoard.items.find((each, index) => {
        if (each.id == itemId) position = index;
        return each.id == itemId;
      });
      selectedBoard.items.splice(position, 1);
      const selectedNewBoard = this.data.find(board => board.title == newBoard);
      selectedItem.board = newBoard;
      selectedNewBoard.items.push(selectedItem);

      this.saveData();
    }
  }
};
</script>

<style>
.board-container {
  display: flex;
}
.new-board input {
  margin-top: 1rem;
  width: 17rem;
  color: rgb(23, 43, 77);
  font-size: 1rem;
  font-weight: 100;
  border: 1px solid #ebecf0;
  transition: all ease 0.2s;
  height: 2rem;
  padding-left: 0.4rem;
  font-family: inherit;
}
.new-board input:focus {
  outline: none;
}
</style>
