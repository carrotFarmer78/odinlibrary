<template>
  <h1 id="header">Library</h1>
  <div class="libraryCard">
  <div class="libraryList" v-if="libraryHasBooks">
    <div>
      The Library has {{ amountOfBooks }} book(s)
      <ul>
        <li v-for="book in myLibrary" :key="book.author">
          <div class="book-info">
            <span><strong>{{ book.title }}</strong> by <strong>{{ book.author }}</strong> </span>
          </div>
          <div class="read-status" :style="{ color: book.isRead ? 'green' : 'red' }">
            {{ book.isRead ? 'read' : 'not read yet' }}
            <button @click="book.isRead = !book.isRead">Mark as (Un)read</button>
            <button @click="removeBook(book)" class="fa-regular fa-trash-can fa-2xl iButton"></button>
          </div>
        </li>
      </ul>

    </div>
  </div>
  <div v-else>
    Currently there are no Books in the Library. Click the "+" to add one
  </div>
  <button class="fa-solid fa-plus fa-2xl iButton" @click="newBookButtonWasClicked = !newBookButtonWasClicked"></button>
  <div class="newBookForm" v-if=(newBookButtonWasClicked)>
        <div>
          <label for="title">Title: </label>
          <input type="text" id="title" v-model="newBook.title" required>
        </div>
        <div>
          <label for="author">Author: </label>
          <input type="text" id="author" v-model="newBook.author" required>
        </div>
        <div>
          <label for="isRead">Did you already read the Book? </label>
          <input type="checkbox" id="isRead" v-model="newBook.isRead">
        </div>
        <button type="submit" @click="addBook(); newBookButtonWasClicked = false">Add to list</button>
      </div>
    </div>
</template>

<script setup>
/* eslint-disable */

import { ref, computed } from 'vue'

const myLibrary = ref([])
const newBookButtonWasClicked = ref(true)

const amountOfBooks = computed(() => {
  console.log(myLibrary.value.length)
  return myLibrary.value.length
})
const libraryHasBooks = computed(() => {
  return amountOfBooks.value > 0 ? true : false
})

function book(title, author, isRead) {
  this.title = title
  this.author = author
  this.isRead = isRead
}
function addToLibrary(book) {
  console.log(book)
  console.log(myLibrary.value)
  myLibrary.value.push(book)
}
function removeBook(book){
myLibrary.value.forEach(element => {
  if(element.title == book.title){
    const index = myLibrary.value.indexOf(element)
    myLibrary.value.splice(index, 1)
  
  }
});
}


const harryPotter = new book("Harry Potter", "JK Rowling", false)
const theHobbit = new book("The Hobbit", "JRR Tolkien", true)

addToLibrary(harryPotter)
addToLibrary(theHobbit)

const newBook = ref({
  title: '',
  author: '',
  isRead: false,
  getTitle(){
    return this.title;
  }
});
function addBook() {
  myLibrary.value.push({ ...newBook.value });
  newBook.value.title = '';
  newBook.value.author = '';
  newBook.value.isRead = false;
}


</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

#header {
  text-align: center;
}

.book-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
}

.book-info {
  flex: 1;
}

.read-status {

  text-align: right;
  margin-right: 50px;
}

.addNewBookButton {
  display: flex;

}

.newBookForm {
  padding: 25px;
  display: flex;
  justify-content: center;
  background-color: rgb(209, 207, 207);
  margin-top: 20px;
  border-radius: 25px;
}

input {
  margin: 20px;
}
.iButton{
  background:none;
  border: none;
}
.libraryCard {
  display: flex;
  flex-direction: column;
  align-items: center; /* Hiermit wird die Karte horizontal zentriert */
  height: 50%; /* Dies sorgt dafür, dass die Karte die volle Bildschirmhöhe einnimmt */
  background-color: rgb(234, 236, 238);
  padding: 30px;
  border-radius: 25px;
  max-width: 1000px;
  margin: 0 auto; /* Dies zentriert die Karte horizontal im Container (falls vorhanden) */
}
li{
  display: flex;
  margin: 15px;
  padding: 5px;
  border-bottom: 2px solid rgb(197, 188, 188);
}
.libraryList{
  width: 100%;
}
</style>
