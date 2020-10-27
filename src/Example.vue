<template>
  <div id="app">
    <div class="container">
      <div class="col">
        <form @submit.prevent="UpdatePromise">
          <label class="form-control">
            <p>delay</p>
            <input type="text" v-model="form.delay" />
          </label>
          <label class="form-control">
            <p>resolve time</p>
            <input type="text" v-model="form.resolve" />
          </label>
          <label class="form-control">
            <p>tag</p>
            <input
              type="text"
              :value="form.tag"
              @change="form.tag = $event.target.value"
            />
          </label>
          <label class="form-control">
            reject
            <input type="checkbox" v-model="form.reject" />
          </label>
           <label class="form-control">
             <button type="submit">Update promise</button>
           </label>
          
        </form>
      </div>
      <div class="col border">
        <promise-vue :delay="form.delay" :promise="example" :tag="form.tag">
          <template v-slot:pendding>
            <div>Loadding...</div>
          </template>

          <template v-slot:resolved="data">
            <pre>resolve === {{ data }}</pre>
          </template>

          <template v-slot:error="error">
            <pre>reject === {{ error }}</pre>
          </template>
        </promise-vue>
      </div>
    </div>
  </div>
</template>

<script>
import VuePromiseComponent from "./components/promise-vue.js";

const testPromise = (time = 1000, reject = false) => {
  return new Promise((res, rej) => {
    setTimeout(() => {
      if (reject)
        return rej({
          status: 404,
          message: "Not Found 404",
        });
      res([
        { id: 1, login: "bot" },
        { id: 2, login: "admin" },
      ]);
    }, time);
  });
};

export default {
  name: "App",
  components: {
    "promise-vue": VuePromiseComponent,
  },
  data() {
    return {
      example: null,
      form: {
        delay: 1000,
        resolve: 1000,
        tag: "div",
        reject: false,
      },
    };
  },
  methods: {
    UpdatePromise() {
      this.example = testPromise(this.form.resolve, this.form.reject);
    },
  },
  created() {
    this.example = testPromise(1000);
  },
};
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  padding: 30px;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 900px;
  margin: 0 auto;
}
.container {
  display: flex;
  margin: 0 -30px;
}
.col {
  max-width: 50%;
  width: 50%;
  flex-basis: 50%;
  padding: 30px;
}
.border {
  border: 1px solid #777;
}
.form-control {
  display: block;
    font-size: 18px;
    margin-bottom: 15px;
}
.form-control input{
      padding: 5px;
}
[type="submit"]{
      border: none;
    padding: 10px 15px;
    cursor: pointer;
    font-size: 18px;
    transition: .3s;
}
[type="submit"]:hover{
  background: #1987e6;
  color: #fff;
}
</style>
