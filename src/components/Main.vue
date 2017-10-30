<template>
    <div class="container">
        <div class="page-header">
            <h1>VF-Firebase Sample Application</h1>
        </div>
    
        <div class="panel panel-default">
            <div class="panel-heading">
                <h3>Add Book</h3>
            </div>
            <div class="panel-body">
                <form id="form" class="form-inline" v-on:submit.prevent="addBook">
                    <div class="form-group">
                        <Label for="bookTitle">Title:</Label>
                        <input type="text" id="bookTitle" placeholder="Title" class="form-control" v-model="newBook.title">
                    </div>
                    <div class="form-group">
                        <Label for="bookAuthor">Author:</Label>
                        <input type="text" id="bookAuthor" placeholder="Author" class="form-control" v-model="newBook.author">
                    </div>
                    <div class="form-group">
                        <Label for="bookUrl">Url:</Label>
                        <input type="text" id="bookUrl" placeholder="Book Url" class="form-control" v-model="newBook.url">
                    </div>
                    <input type="submit" class="btn btn-primary" value="Add Book"> <br>
                </form>
            </div>
        </div>

        <div class="panel panel-default">
            <div class="panel-heading">
                  <h3>Books Lists</h3>
            </div>
            <div class="panel-body">
                <table class="table tabel-striped">
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
                        </tr>
                    </thead>
                    <tbody class="text-left">
                        <tr v-for="book in books">
                            <td>
                                <a v-bind:href="book.url">{{ book.title }}</a>
                            </td>
                            <td>
                                {{ book.author }}
                            </td>
                            <td>
                                <span class="glyphicon glyphicon-trash btn btn-danger" v-on:click="removeBook(book)"></span>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>


    </div>
</template>

<script>
import firebase from 'firebase'
import toastr from 'toastr'
// Initialize Firebase
  var config = {
    apiKey: "AIzaSyACunjUtZa2MLdC0rC4vyf81PtHUhpp7oE",
    authDomain: "global-d7dbc.firebaseapp.com",
    databaseURL: "https://global-d7dbc.firebaseio.com",
    projectId: "global-d7dbc",
    storageBucket: "global-d7dbc.appspot.com",
    messagingSenderId: "989774799424"
};

let app = firebase.initializeApp(config)
let db = app.database()
let booksRef = db.ref('books')

export default {
    name: 'Main',
    firebase: {
        books: booksRef
    },
    data () {
        return {
            newBook: {
                title: '',
                author: '',
                url: '',
            }
        }
    },
    methods: {
        addBook: function() {
            booksRef.push(this.newBook)
            this.newBook.title = ''
            this.newBook.author = ''
            this.newBook.url = ''
            toastr.success('Book Added!')
        },
        removeBook: function(book) {
            booksRef.child(book['.key']).remove()
            toastr.error('Book has been remove!')
        }
    }
}
</script>

<style>

</style>

