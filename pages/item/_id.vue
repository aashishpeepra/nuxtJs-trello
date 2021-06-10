<template>
  <div class="cover">
    <div class="description">
      <div class="description_head">
        <input
          type="text"
          id="title"
          v-model="item.title"
          placeholder="Title "
        />
      </div>
      <div class="description_middle">
        <div>
          <label for="status">Status</label>
          <select id="status" v-model="item.status">
            <option
              v-for="each in Constants.status"
              :key="each"
              :value="each"
              >{{ each }}</option
            >
          </select>
        </div>
        <div>
          <label for="priority">Priority</label>
          <select id="priority" v-model="item.priority">
            <option
              v-for="each in Constants.priority"
              :key="each"
              :value="each"
              >{{ each }}</option
            >
          </select>
        </div>
      </div>
      <div class="description_end">
        <label for="Description">Description</label>
        <textarea v-model="item.desc" id="Description"></textarea>
      </div>
      <div class="description_final">
        <button type="button" id="save" @click="saveItem">Save</button>
        <button type="button" id="save" @click="$router.push('/')">
          Go Back
        </button>
         <button type="button" id="save" @click="deleteItem">
          Delete 
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Constants from "../../helper/constants";
import localforage from "localforage";

export default {
  async asyncData({ params, query, redirect }) {
    return { id: params.id, board: query.board, redirect };
  },
  beforeCreate() {
    localforage.getItem("data").then(response => {
      console.log(response);
      let findBoard = response.find(board => board.title == this.board);
      console.log(findBoard);
      if (!findBoard) {
        this.$router.push("/");
        alert("Board doesn't exists");
      }
      let findItem = findBoard.items.find(item => item.id == this.id);
      if (!findItem) {
        this.$router.push("/");
        alert("Item doesn't exists");
        return;
      }

      this.item = findItem;
      this.wholeData = response;
    });
  },
  methods: {
    deleteItem: function() {
      const selectedBoard = this.wholeData.find(board => board.title == this.board);
      let position = 0;
      selectedBoard.items.find((each, index) => {
        if (each.id == this.id) position = index;
        return each.id == this.id;
      });
      selectedBoard.items.splice(position, 1);
      localforage.setItem("data",this.wholeData);
      this.$router.push("/");
    },
    saveItem: function() {
      let findBoard = this.wholeData.find(board => board.title == this.board);

      let findItem = findBoard.items.find(item => item.id == this.id);

      Object.keys(this.item).map(eachKey => {
        findItem[eachKey] = this.item[eachKey];
      });
      localforage.setItem("data", this.wholeData);
      this.$router.push("/");
    }
  },
  data: function() {
    return {
      wholeData: [],
      item: {
        title: "",
        status: "Started",
        priority: "P1",
        desc: "",
        id: "",
        board: ""
      },
      Constants: Constants
    };
  }
};
</script>

<style>
.cover {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.description {
  height: 30rem;
  width: 40rem;
  background-color: #fff;
  border-radius: 5px;
  padding: 1rem;
}
.description_head input {
  width: 90%;
  color: rgb(23, 43, 77);
  font-size: 1.25rem;
  font-weight: 600;
  background-color: inherit;
  transition: all ease 0.2s;
  font-family: inherit;
  border: 0px;
}
.description_head input:hover {
  cursor: pointer;
}
.description_head input:focus {
  outline: none;
}
.description_middle {
  margin: 1rem 0rem;
}
.description_middle div label {
  margin-bottom: 0.2rem;
}
.description_middle div {
  display: flex;
  flex-direction: column;
  margin: 1rem 0rem;
}
.description_end {
  display: flex;
  flex-direction: column;
}
.description_final {
  margin-top: 2rem;
}
.description_final button {
  padding: 0.2rem 1rem;
}
</style>
