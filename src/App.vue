<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 & Firebase Sample App</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" name="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" name="bookAuthor" class="form-control" v-model="newBook.author">
          </div>
          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" name="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <div v-if="!edit">
            <input type="submit" class="btn btn-primary" value="Add Book">
          </div>
          <div v-else>
            <input type="button" class="btn btn-info" value="Update Book">
            <input type="button" class="btn btn-warning" value="Cancel">
          </div>
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books List</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Title
              </th>
              <th>
                Author
              </th>
              <th>
                Delete
              </th>
              <th>
                Edit
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td>
                <a v-bind:href="book.url">{{book.title}}</a>
              </td>
              <td>
                {{book.author}}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" @click="removeBook(book)"></span>
              </td>
              <td>
                <span class="glyphicon glyphicon-pencil" @click="editBook(book)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello'

import Firebase from 'firebase'

let config = {
  apiKey: "AIzaSyDIOqOEG_-r4-KqJiGDU5jip-Sd2g4Aoa8",
  authDomain: "vuejs-firebase-01-33993.firebaseapp.com",
  databaseURL: "https://vuejs-firebase-01-33993.firebaseio.com",
  storageBucket: "vuejs-firebase-01-33993.appspot.com",
  messagingSenderId: "762224435246"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let bookRef = db.ref('books');

export default {
  name: 'app',
  firebase:{
    books: bookRef
  },
  data(){
    return{
      edit: false,
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function(){
      bookRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },
    removeBook: function(book){
       bookRef.child(book['.key']).remove();
    },
    editBook: function(book){
       this.newBook.title = book.title;
       this.newBook.author = book.author;
       this.newBook.url = book.url ;
       this.edit = true;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
