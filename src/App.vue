<script>
export default {
  data() {
    return {
      value: "",
      todoList: JSON.parse(localStorage.getItem("todolist")) ?? [],
    };
  },

  methods: {
    addToList() {
      if (this.value !== "") {
        this.todoList.push({
          id: Date.now(),
          item: this.value,
          checked: false,
        });
        this.value = "";
      }
      this.addToLocalStorage();
    },
    changeCheck(id) {
      this.todoList.forEach((el, index) => {
        if (el.id === id) {
          el = {
            id: id,
            item: el.item,
            checked: !el.checked,
          };
          this.todoList[index] = el;
        }
      });
      this.addToLocalStorage();
    },
    removeItem(id) {
      this.todoList = this.todoList.filter((i) => i.id !== id);
      this.addToLocalStorage();
    },
    addToLocalStorage() {
      localStorage.setItem("todolist", JSON.stringify(this.todoList));
    },
  },
  computed: {},
  created() {},
  mounted() {},
  // ...
};
</script>

<template>
  <div class="todo-card">
    <h1>Todo list</h1>
    <div class="flex">
      <input type="text" v-model="value" placeholder="Add item" />
      <button @click="addToList">Add</button>
    </div>
    <ul>
      <li
        v-for="object in todoList"
        :style="
          object.checked
            ? {
                'background-color': '#8ec5fc',
                'background-image':
                  'linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%)',
              }
            : ''
        "
      >
        <font-awesome-icon
          icon="fa-regular fa-square-check"
          v-if="object.checked"
          @click="changeCheck(object.id)"
          class="check-box"
        />
        <font-awesome-icon
          icon="fa-regular fa-square"
          v-else
          @click="changeCheck(object.id)"
          class="check-box"
        />
        {{ object.item }}
        <font-awesome-icon
          icon="fa-solid fa-xmark"
          @click="removeItem(object.id)"
          class="close"
        />
      </li>
    </ul>
  </div>
</template>

<style scoped>
.flex {
  display: flex;
  margin-bottom: 2rem;
  gap: 1rem;
}

.todo-card {
  background-color: white;
  color: #695c68;
  width: 100%;
  max-width: 35rem;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 2rem 2rem rgba(0, 0, 0, 0.181);
  display: flex;
  flex-direction: column;
  height: 80vh;
}

.todo-card h1 {
  text-align: center;
  margin-bottom: 2rem;
}

input {
  color: #695c68;
  outline: none !important;
  border-radius: 1rem;
  padding: 1rem;
  width: 100%;
  border: 0.2rem solid #90618d;
}

input::placeholder {
  color: #917791;
}

button {
  width: 40%;
  border: none;
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
  color: #695c68;
  border-radius: 1rem;
  padding: 1rem;
  cursor: pointer;
  box-shadow: 0 1rem 2rem rgba(0, 0, 0, 0.181);
}

button:hover {
  background-color: #e0c3fc;
  background-image: linear-gradient(62deg, #e0c3fc 0%, #8ec5fc 100%);
  box-shadow: 0 1rem 2rem rgba(0, 0, 0, 0.246);
}

ul {
  list-style-type: none;
  overflow-y: scroll;
}
ul::-webkit-scrollbar {
  display: none;
}

li {
  background-color: #c5def7;
  padding: 1.5rem;
  border-radius: 1rem;
  margin-bottom: 1rem;
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.093);
}

li:hover {
  background-color: #98c8f7;
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.148);
}

.after-check,
.after-check:hover {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}

.check-box {
  float: left;
  margin-right: 1rem;
}

.close {
  float: right;
}
</style>
