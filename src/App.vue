<template>
  <div id="app">
    <!-- App Introduction -->
    <h1 class="text-center my-2 mt-4 text-primary">Wonderfull Quotes Poject</h1>
    <p class="text-center text-muted">😀 Second Project With VueJS 😀</p>

    <!-- Quotes Capecity -->
    <div class="container-fluid mt-5">
      <div class="py-2 row">
        <div class="col-sm-10 offset-sm-1">
          <p class="text-center text-warning">Quotes Capecity</p>
          <div class="progress">
            <div
              class="progress-bar"
              :class="quotes.length >= 7 ? 'bg-danger' : 'bg-success'"
              role="progressbar"
              :style="{ width: `${quotes.length}0%` }"
            >{{ quotes.length }}/10</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Quote Form Component -->
    <template v-if="quotes.length >= 10">
      <div class="container-fluid">
        <div class="row">
          <div class="col-sm-10 offset-sm-1">
            <div
              class="alert alert-danger text-center"
              role="alert"
            >You Exceeded All Quotes You Can Write</div>
          </div>
        </div>
      </div>
    </template>
    <template v-else>
      <QuoteForm :formSubmit="this.formSubmit" />
    </template>

    <!-- Quote Container -->
    <div class="container-fluid">
      <div class="py-2 row">
        <div class="col-sm-12">
          <p class="text-center text-warning">Quotes</p>
          <!-- Quote Component -->
          <Quote
            v-for="(quote, index) in quotes"
            :key="index"
            @click.native="quotes.splice(index, 1)"
          >
            <h5 slot="title" class="card-title text-center bg-dark text-white p-1">{{ quote.title }}</h5>
            <p slot="body" class="card-text font-weight-bold">{{ quote.body }}</p>
            <p
              slot="writter"
              class="text-muted text-right"
              style="font-style: italic"
            >{{ quote.writter }}</p>
          </Quote>
        </div>
        <template v-if="quotes.length >= 1">
          <div class="col-sm-10 offset-sm-1">
            <div
              class="alert alert-info text-center"
              role="alert"
            >You Can Delete a Quote by Pressing it</div>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import Quote from "./components/Quote";
import QuoteForm from "./components/QuoteForm";

export default {
  name: "App",
  data() {
    return {
      quotes: [
        {
          title: "Famous Chinese Quote",
          body: "God Helpes Those Who Help Themselves.",
          writter: "Idiom",
        },
      ],
    };
  },
  methods: {
    formSubmit(formProps) {
      this.quotes = [formProps, ...this.quotes];
    },
  },
  components: {
    Quote,
    QuoteForm,
  },
};
</script>

<style>
body {
  background-color: black;
  color: white;
}
#app {
  font-family: "Ubuntu", Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
