<template>
  <div id="app" class="app">
    <!-- heading -->
    <header>
      <h1 class="app__heading">Books<span>.app</span></h1>
    </header>
    <books-list :myBooks="books" @remove="removeBook" />
    <!-- zadanie 1 -->
    <div>
      <p>{{booksAmount}}</p>
    </div>
    <!-- add book form -->
    <books-length-msg :nbOfBooks="books.length"/>
    <books-form @addBook="addBook"/>
    <books-summary :nbOfBooks="books.length" :totalPrice="booksPrice"/>
  </div>
</template>
<script>
import BooksList from './components/BooksList'
import BooksLengthMsg from './components/BooksLengthMsg'
import BooksForm from './components/BooksForm'
import BooksSummary from './components/BooksSummary'

export default {
  name: 'App',
  data: () => ({
    books: []
  }),
  methods: {
    addBook (newBook) {
      this.books.push(newBook)
    },
    removeBook (i) { this.books.splice(i, 1) }
  },
  components: {
    BooksList,
    BooksLengthMsg,
    BooksForm,
    BooksSummary
  },
  computed: {
    booksPrice () {
      let totalPrice = 0
      for (let i = 0; i < this.books.length; i++) {
        totalPrice += parseInt(this.books[i].price)
      }
      return totalPrice
    }
  },
  created () {
    fetch(
      'https://openlibrary.org/api/books?bibkeys=ISBN:9789123963140,ISBN:9780030234491,ISBN:9780320037825&format=json&jscmd=data')
      .then(response => response.json())
      .then(data => {
        for (let i = 0; i < 3; i++) {
          this.books.push({ title: data[Object.keys(data)[i]].title, price: 20 })
        }
      })
  }
}
</script>

<style lang="scss" scoped>

.app {
  width: 100%;
  max-width: 1000px;
  padding: 2rem;
  margin: 0 auto;

  &__heading {
    font-size: 3rem;
    text-align: center;
    span {
      color: #5a58da;
    }
  }
}
</style>
