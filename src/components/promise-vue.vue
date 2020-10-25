<template>
  <component v-if="isShow" :is="tag ? tag : 'div'">
    <slot v-if="data && resolved" name="resolved" :data="data" ></slot>
    <slot v-if="!resolved" name="pendding"></slot>
    <slot v-if="error && resolved" name="error" :data="error"/>
  </component>
</template>
<script>
export default {
  name: "VuePromiseComponent",
  props: {
    tag: {
      type: String,
      default: 'div'
    },
    delay: {
      type: [Number, String],
      default: 0
    },
    promise: {
      validator: (p) => {
        return (
          p && typeof p.then === "function" && typeof p.catch === "function"
        );
      },
    },
  },
  data() {
    return {
      data: null,
      resolved: false,
      error: false,
      timerId: null,
      isShow: true
    };
  },
  methods: {
    resetState() {
      this.data = null;
      this.resolved = false;
      this.error = false;
      if(this.timerId) {
        clearTimeout(this.timerId)
      }
       this.timerId = null
    },
    setDelay(time){
      if (this.timerId){
        clearTimeout(this.timerId)
      }
      if(!time){
        this.isShow = true;
      }else{
         this.isShow = false;
         this.timerId = setTimeout(
          () => {
            this.isShow = true
          },
          time,
        )
      }
    }
  },
  watch: {
    promise: {
      immediate: true,
      handler(promise) {
        this.resetState();
        this.setDelay(this.delay);
        promise
          .then((data) => {
            this.resetState();
            this.data = data;
          })
          .catch((error) => {
            this.resetState();
            this.error = { error };
          })
          .finally(() => {
             this.isShow = true;
            this.resolved = true;
          });
      },
    },
  },
};
</script>