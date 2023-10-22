<template>
  <div id="app">
    <div class="syntax">
      <h1>{{ syntax }}</h1>

      <h2>{{ num + 2 }}</h2>
      <h3>{{ num > 1 ? "Number > 1" : "Number < 1" }}</h3>
      <h3>{{ obj.age }}</h3>
      <h3>{{ obj }}</h3>
    </div>

    <div class="binding">
      <h1>{{ binding }}</h1>

      <!--если obj то выведет весь obj ключ значение-->
      <a :href="linkUlr">Link 1</a>
      <!---->
      <a :[attrKey]="linkUlr"> Link 2</a>
    </div>

    <div class="v-if">
      <h1>{{ vif }}</h1>
      <!--v-if-->
      <!--v-else-if-->
      <!--v-else-->
      <!-- <div v-if="msgIsVisible === true" class="message">Some message!</div>
    <div v-else-if="msgIsVisible === false" class="message">v else message</div>
    <div v-else class="message">Another message!</div> -->

      <template v-if="msgIsVisible === 1">
        <div class="message">Some message!</div>
      </template>

      <template v-else-if="msgIsVisible === 2">
        <div class="message">v else message</div>
      </template>

      <template v-else>
        <div class="message">Another message!</div>
      </template>

      <!--v-show просто скрываеться стилями не работает с v-if & v-else-->
      <div class="message" v-show="msgIsVisibleBlock">V show example</div>
    </div>

    <div class="events">
      <h1>{{ events }}</h1>

      <div class="name-input" v-if="inputNameVisible">
        <label>Name</label>
        <input type="text" placeholder="input-name" key="name-input" />
      </div>

      <div class="nickname-input" v-else>
        <label>Nickname</label>
        <input type="text" placeholder="input-nickname" key="nickname-input" />
      </div>

      <button @click="inputNameVisible = !inputNameVisible">Change form</button>

      <button class="btn" @click="onClick('some value', $event)">
        Clickevent
      </button>

      <a :href="linkUlr" @click.prevent="onLinkClick">Link</a>

      <input type="text" @keyup.enter="onKeyUp" />
      <h1>{{ text }}</h1>
    </div>

    <div class="v-for">
      <h1>{{ vfor }}</h1>
      <ul>
        <!-- <li v-for="(color, index) in colors" :key="index">
          {{ index + 1 }} {{ color }}
        </li> -->

        <!-- <li v-for="user in users" :key="user.id">
          {{ user.name }} : {{ user.age }}
        </li>
      </ul> -->

        <li v-for="(value, name, index) in product" :key="index">
          {{ index }} {{ name }} : {{ value }}
        </li>
      </ul>

      <div>
        <!-- <input type="text" @keyup.enter="addNewColor" /> -->
        <input type="text" @keyup.enter="deleteProp" />
      </div>
    </div>

    <div class="computed">
      <h1>Hello {{ fullName }}</h1>

      <input type="text" @keyup.enter="setName" />
    </div>

    <div class="classes">
      <h1>{{ classes }}</h1>

      <h2
        class="optional"
        :class="[classObj, activeClass]"
        :style="{ 'font-size': fontSize + 'px' }"
      >
        Hello World
      </h2>
    </div>

    <HelloWorld
      :title="title"
      :user-object="user"
      class="emit"
      @onChangeCounter="onChangeCounterInComponets"
    />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",

  components: { HelloWorld },

  data: () => ({
    syntax: "Syntax",
    binding: "Binding",
    vif: "V-if",
    events: "Events",
    vfor: "V-for",
    classes: "Class",

    num: 2,
    obj: {
      age: 36,
    },

    linkUlr: "https://google.com",
    attrKey: "href",
    inputName: true,
    msgIsVisible: 1,
    msgIsVisibleBlock: true,
    inputNameVisible: true,

    //example events
    text: "",

    colors: ["orange", "black", "yellow"],

    users: [
      { name: "Serhii", age: 36, id: 1 },
      { name: "Ivan", age: 33, id: 2 },
    ],

    product: {
      brand: "Apple",
      model: "Iphon 11 Pro",
      price: "$1000",
    },

    //example computed
    firstName: "",
    lastName: "",

    // firstName: "Serhii",
    // lastName: "Hul",

    //example classes
    activeClass: "my-active-class",
    isActive: true,
    fontSize: 30, // реже используется style

    title: "Some title",

    user: {
      name: "Serhii",
    },
  }),

  //example methods
  methods: {
    onClick(value, e) {
      console.log(value, e);
    },

    onLinkClick() {
      console.log("link click");
    },

    onKeyUp(e) {
      console.log("on key up", e);
      this.text = e.target.value;
    },

    addNewColor(e) {
      // this.colors.push(e.target.value);
      // this.colors[this.colors.length] = e.target.value; - не отслеживает
      this.$set(this.colors, this.colors.length, e.target.value); // такая запись в основном делается для объектов
    },

    deleteProp(e) {
      // delete this.product[e.target.value];
      this.$delete(this.product, e.target.value);
    },

    setName(e) {
      // this.firstName = e.target.value;
      this.fullName = e.target.value;
    },

    onLastNameUpdate(value) {
      console.log("watch", value);
    },

    //example emit
    onChangeCounterInComponets(value) {
      console.log("In App.vue, counter:", value);
    },
  },

  //example computed - кэшируется
  computed: {
    // fullName() {
    //   return `${this.firstName || "Default"} ${this.lastName || "User"}`;
    // },

    fullName: {
      get() {
        return `${this.firstName || "Default"} ${this.lastName || "User"}`;
      },

      set(value) {
        console.log(value);

        const [firstName, lastName] = value.split(" ");
        console.log(firstName, lastName);

        this.firstName = firstName;
        this.lastName = lastName;
      },
    },

    //example classes

    classObj() {
      return {
        "--active": this.isActive,
        error: !this.isActive,
      };
    },
  },

  //example watch
  watch: {
    lastName: "onLastNameUpdate",
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  margin-top: 20px;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin: 5px 0 10px;
}

a {
  color: #fff;
}

ul {
  margin: 0;
}

button,
input {
  width: 100px;
}

label {
  padding-right: 5px;
}

.syntax,
.binding,
.v-if,
.events,
.v-for,
.computed,
.classes,
.emit {
  width: 500px;
  min-height: 200px;
  margin: 0 auto 10px;
  background: linear-gradient(180deg, #8078ff, #4b52f1);
  border: 1px solid #272630;
  border-radius: 8px;
  box-shadow: 2px 2px 0 0 rgba(39, 38, 48, 0.8);
}

.events,
.binding {
  display: flex;
  flex-flow: column;
  justify-content: space-evenly;
  align-items: center;
}

.message,
.name-input {
  margin: 0 0 10px;
}

.--active {
  font-size: 18px;
  font-family: "Courier New", Courier, monospace;
  color: #272630;
}

.error {
  color: red;
  font-family: Georgia, "Times New Roman", Times, serif;
}
</style>
