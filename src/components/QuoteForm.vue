<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-8 offset-sm-2 py-2 my-2 border">
        <p class="text-center text-warning">Add New Quote</p>
        <form @submit="this.sendObject">
          <div class="form-group">
            <label for="quote-title">Quote Title</label>
            <input type="text" class="form-control" id="quote-title" v-model="title" />
          </div>
          <div class="form-group">
            <label for="quote-body">The Quote</label>
            <textarea class="form-control" id="quote-body" v-model="body"></textarea>
          </div>
          <div class="form-group">
            <label for="quote-writter">Quote Author/Writter</label>
            <input type="text" class="form-control" id="quote-writter" v-model="writter" />
          </div>
          <button
            type="submit"
            class="btn btn-primary"
            :disabled="title === '' || body === '' || writter ===''"
          >Add Quote</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      body: "",
      writter: "",
    };
  },
  methods: {
    sendObject(e) {
      e.preventDefault();
      this.formSubmit({
        title: this.title,
        body: this.body,
        writter: this.writter,
      });
    },
  },
  props: ["formSubmit"],
};
</script>

<style>
</style>