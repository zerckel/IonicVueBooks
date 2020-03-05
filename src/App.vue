<template id="home">
  <div class="ion-page">
    <ion-header>
      <ion-toolbar>
        <ion-title>Books Manager</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <ion-card v-for="book in books" v-bind:key="book.isbn" class="card">
        <ion-card-header>
          <ion-card-subtitle>ISBN: {{book.isbn}}</ion-card-subtitle>
          <ion-card-title>{{book.title}} <ion-card-subtitle>By {{book.author}}</ion-card-subtitle></ion-card-title>
          <ion-card-subtitle>{{book.pageCount}} <span v-if="book.pageCount === 1">page</span><span v-else>pages</span></ion-card-subtitle>
        </ion-card-header>
        <ion-img style="height: 35vh" :src="book.thumbnailUrl"></ion-img>
        <ion-card-content>
          {{book.shortDescription}}
        </ion-card-content>
        <ion-button @click="modifyBook(book.isbn)" expand="block" color="success">Modify Book Information</ion-button>
        <ion-button @click="deleteBook(book.isbn)" expand="block" color="danger">Delete Book</ion-button>
      </ion-card>
    </ion-content>
    <ion-fab vertical="bottom" horizontal="end" slot="fixed">
      <ion-fab-button @click="addBook">
        <ion-icon name="add"></ion-icon>
      </ion-fab-button>
    </ion-fab>
  </div>
</template>

<script>
  import Modal from '../src/components/modal.vue'
  export default {
    name: "app",
    data() {
      return {
        books: undefined
      }
    },
    methods: {
      deleteBook: function (isbn) {
        this.books = this.books.filter(function (book) {
          return book.isbn !== isbn
        });
        this.updateStorage()
      },
      updateStorage: function () {
        localStorage.setItem('books', JSON.stringify(this.books))
      },
      addBook: function () {
        this.$ionic.modalController
                .create({
                  component: Modal
                })
                .then(m => m.present())
        this.waitClosing()
      },
      modifyBook: function (isbn) {
        let book = this.books.filter(function (book) {
          return book.isbn === isbn
        });
        book = book[0]
        this.$ionic.modalController
                .create({
                  component: Modal,
                  componentProps:{
                    data:{
                      book: {
                        author: book.author,
                        pageCount: book.pageCount,
                        shortDescription: book.shortDescription,
                        thumbnailUrl: book.thumbnailUrl,
                        title: book.title,
                        isbn: book.isbn
                      }
                    },
                    propsData:{
                      modify: true
                    }
                  }
                })
                .then(m => m.present())
        this.waitClosing()
      },
      waitClosing: function () {
        let vm = this
        window.addEventListener('ionModalDidDismiss', function (data) {
          data.detail.data !== null ? vm.pushBook(data.detail.data) : false
        })
      },
      pushBook: function (book) {
        let found = this.books.some(el => el.isbn === book.isbn)
        found ? this.deleteBook(book.isbn) : false
          this.books.unshift(book)
      }
    },
    created: function () {
      if (localStorage.getItem('books') === null) {
        let vm = this
        fetch('api/books.json')
                .then(function (response) {
                  response.json()
                          .then(function (value) {
                            localStorage.setItem('books', JSON.stringify(value))
                            vm.books = value
                          });
                });
      } else {
        this.books = JSON.parse(localStorage.getItem('books'))
      }
    },
    watch:{
      books: function () {
        this.updateStorage()
        console.log('check')
      }
    }
  }
</script>