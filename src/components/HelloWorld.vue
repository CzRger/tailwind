<template>
  <div class="wrapper">
    <div class="top">
      <img class="img" src="@/logo.svg" alt="Todo" />
      <div class="title">待办</div>
    </div>
    <form @submit="newItemSubmitHandler" class="form">
      <input
        class="input"
        type="text"
        @change="handleItemInput"
        :value="state.pendingItem"
        placeholder="填写事项"
      />
      <button class="button">
        +
      </button>
    </form>

    <div class="li" v-for="(item, index) in state.list" :key="index">
      <template v-if="item.isEditing">
        <input
          class="input"
          type="text"
          :value="item.name"
          @change="e => toggleIsEditingAt(e, index)"
        />
        <button class="action button" @click="item.isEditing = false">
          保存
        </button>
      </template>
      <template v-else>
        <span class="todoInput" @click="item.isEditing = true">{{item.name}}</span>
        <button class="button" @click="removeItemAt(item.id)">
          －
        </button>
      </template>
    </div>
  </div>
</template>

<script>
import '../styles/App.css';

export default {
  name: 'HelloWorld',
  data() {
    return {
      state: {
        list: [],
        pendingItem: ""
      },
      lastItemId: 0
    }
  },
  methods: {
    newItemSubmitHandler(e) {
      e.preventDefault();
      const id = this.newItemId()
      this.state = {
        list: [
          {
            name: this.state.pendingItem,
            isEditing: false,
            id
          },
          ...this.state.list
        ],
        pendingItem: ""
      }
    },
    newItemId() {
      const id = this.lastItemId
      this.lastItemId += 1
      return id
    },
    toggleIsEditingAt(e, index) {
      this.state.list[index].name = e.target.value
    },
    removeItemAt(id) {
      const newList = this.state.list.filter(item => id !== item.id)
      this.state.list = newList
    },

    handleItemInput(e) {
      this.state.pendingItem = e.target.value
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
