<template>
  <div class="hello">
    <h1>{{ emitsEvent }}</h1>

    <h2>{{ title }}</h2>
    <button @click="onClick">Send Event</button>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  props: {
    title: {
      type: String,
      default: "Default title",
      // required: true,
    },

    userObject: {
      type: Object,
      // default: () => ({}),
      validator(value) {
        console.log(value);
        return value.name;
      },
    },
  },

  data: () => ({
    emitsEvent: "Emit",

    counter: 0,
  }),

  methods: {
    onClick() {
      // console.log("Click");
      this.counter += 1;
      this.$emit("onChangeCounter", this.counter);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;

  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
