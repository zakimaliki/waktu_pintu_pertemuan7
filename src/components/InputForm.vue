<template>
  <div>
    <label for="fname">Judul Buku</label>
    <input
      type="text"
      id="fname"
      name="bookTitle"
      placeholder="Book Title.."
      v-model="bookData.bookTitle"
    />
    <label for="lname">Author</label>
    <input
      type="text"
      id="lname"
      name="bookAuthor"
      placeholder="Book Author.."
      v-model="bookData.bookAuthor"
    />
    <label for="status-1">
      <input
        type="radio"
        id="status-1"
        value="finnished"
        v-model="bookData.status"
      />
      Selesai Dibaca
    </label>
    <label for="status-2">
      <input
        type="radio"
        id="status-2"
        value="notFinnished"
        v-model="bookData.status"
      />
      Belum Selesai Dibaca
    </label>
    <button type="button" @click="addBookData">Submit</button>
  </div>
</template>

<script>
import { eventBus } from "../main.js";
export default {
  data() {
    return {
      bookData: {
        bookTitle: "",
        bookAuthor: "",
        status: "finnished"
      }
    };
  },
  methods: {
    postData() {
      this.$http
        .post(
          "https://pivotal-expanse-363907-default-rtdb.asia-southeast1.firebasedatabase.app/booklist.json",
          this.bookData
        )
        .then(
          response => {
            this.$router.push("/");
          },
          error => {
            console.log(error);
          }
        );
    },
    addBookData() {
      this.$http
        .get(
          "https://pivotal-expanse-363907-default-rtdb.asia-southeast1.firebasedatabase.app/booklist.json"
        )
        .then(response => {
          return response.json();
        })
        .then(data => {
          const resultArray = [];
          for (let key in data) {
            resultArray.push(data[key]);
          }
          this.bookList = resultArray;
          if (this.bookList.length === 0) {
            this.bookData.id = 1;
            this.postData();
            return;
          }
          this.bookData.id = this.bookList[this.bookList.length - 1].id + 1;
          this.postData();
        });
    }
  }
};
</script>

<style scoped>
label {
  display: block;
  width: 30%;
  margin: 0px auto;
}

label,
input {
  margin: 8px auto;
}

input[type="text"] {
  width: 30%;
  padding: 12px 20px;
  margin: 8px auto;
  display: block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

button {
  background-color: #4caf50;
  color: white;
  padding: 14px 20px;
  margin: 8px auto;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  width: 30%;
  display: block;
}

input[type="submit"]:hover {
  background-color: #45a049;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}
</style>
