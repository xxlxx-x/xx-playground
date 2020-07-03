<template>
  <div id="app">
    <h1>to-do list</h1>
    <form id="form" @submit="onSubmit">
      <input
        type="text"
        id="todo"
        v-model="todoInput"
        class="form__input"
        placeholder="add a todo list"
        name="to-do"
        size="30"
        required
      />
      <button class="form__button" @click="addItem">
        <span>add item</span>
      </button>
    </form>
    <div class="todo-container">
      <ul class="todo-list">
        <template v-for="item in todoList">
          <li :key="item.id">
            <div :class="`todo-name ${item.mightDoneIt}`">
              {{ item.name }}
            </div>
            <div class="spacer"></div>
            <div class="todo-action">
              <img src="@/assets/edit-48.png" />
              <img
                src="@/assets/easy-48.png"
                @click="doneItem(item)"
                @mouseenter="mightDoneIt(item, true)"
                @mouseleave="mightDoneIt(item, false)"
              />
            </div>
          </li>
        </template>
      </ul>
    </div>
  </div>
</template>
<script>
import { ref, watchEffect } from "vue";
export default {
  name: "main",
  setup() {
    let lists = localStorage["todos"];
    // eslint-disable-next-line no-extra-boolean-cast
    lists = Boolean(lists) ? JSON.parse(lists) : [];
    let todoInput = ref("");

    let todoList = ref(lists);
    watchEffect(() => {
      localStorage["todos"] = JSON.stringify(todoList.value);
    });

    function addItem() {
      if (todoInput.value.trim() === "") return;
      const id = new Date().getTime();
      todoList.value.push({
        id,
        name: todoInput.value
      });
      todoInput.value = "";
    }
    function onSubmit(event) {
      event.preventDefault();
    }
    function mightDoneIt(item, showLine) {
      item.mightDoneIt = showLine ? "might-done" : "";
    }
    function doneItem(item) {
      for (const index in todoList.value) {
        if (item.id === todoList.value[index].id) {
          return todoList.value.splice(index, 1);
        }
      }
    }
    return {
      todoInput,
      todoList,
      addItem,
      onSubmit,
      doneItem,
      mightDoneIt
    };
  }
};
</script>

<style lang="scss">
#app {
  overflow: auto;
  height: 100%;
  color: #f38181;
  // background-color: #f38181;
  h1 {
    font-weight: 200;
    text-align: center;
    letter-spacing: 5px;
    font-size: 6rem;
  }
  #form {
    width: 60%;
    margin: auto;
    .form__input {
      width: calc(100% - 100px);
      color: hsla(260, 2%, 25%, 0.7);
      font-size: 1rem;
      font-family: "Gochi Hand", cursive;
      box-sizing: border-box;
      background-color: transparent;

      padding: 0.5rem;
      margin-bottom: 20px;
      margin-right: 10px;
      border-radius: 4px;
      border: solid 3px transparent;
      border-bottom: dashed 3px #fce38a;
      &:focus {
        outline: none;
        border: solid 3px #fce38a;
      }
    }
    .form__button {
      // transform: rotate(4deg);
      font-family: "Gochi Hand", cursive;
      text-decoration: none;
      padding: 0;
      padding-bottom: 3px;
      outline: 0;
      border: none;
      border-radius: 5px;
      background-color: hsla(166, 100%, 50%, 0.7);
      background-image: url("data:image/gif;base64,R0lGODlhBAAEAIABAAAAAAAAACH/C1hNUCBEYXRhWE1QPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS4wLWMwNjEgNjQuMTQwOTQ5LCAyMDEwLzEyLzA3LTEwOjU3OjAxICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOnhtcE1NPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvbW0vIiB4bWxuczpzdFJlZj0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL3NUeXBlL1Jlc291cmNlUmVmIyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ1M1LjEgV2luZG93cyIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo5NUY1OENCRDdDMDYxMUUyOTEzMEE1MEM5QzM0NDVBMyIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo5NUY1OENCRTdDMDYxMUUyOTEzMEE1MEM5QzM0NDVBMyI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjk1RjU4Q0JCN0MwNjExRTI5MTMwQTUwQzlDMzQ0NUEzIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjk1RjU4Q0JDN0MwNjExRTI5MTMwQTUwQzlDMzQ0NUEzIi8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+Af/+/fz7+vn49/b19PPy8fDv7u3s6+rp6Ofm5eTj4uHg397d3Nva2djX1tXU09LR0M/OzczLysnIx8bFxMPCwcC/vr28u7q5uLe2tbSzsrGwr66trKuqqainpqWko6KhoJ+enZybmpmYl5aVlJOSkZCPjo2Mi4qJiIeGhYSDgoGAf359fHt6eXh3dnV0c3JxcG9ubWxramloZ2ZlZGNiYWBfXl1cW1pZWFdWVVRTUlFQT05NTEtKSUhHRkVEQ0JBQD8+PTw7Ojk4NzY1NDMyMTAvLi0sKyopKCcmJSQjIiEgHx4dHBsaGRgXFhUUExIREA8ODQwLCgkIBwYFBAMCAQAAIfkEAQAAAQAsAAAAAAQABAAAAgYEEpdoeQUAOw==");
      box-shadow: 0 2px 0 hsl(198, 1%, 29%);
      transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      transform-origin: center;
      &:active {
        transform: translateY(4px);
        padding-bottom: 0px;
      }
      span {
        background: #f1f5f8;
        display: block;
        padding: 0.5rem 1rem;
        border-radius: 5px;
        border: 2px solid hsl(198, 1%, 29%);
      }
    }
  }
  .todo-container {
    width: 60%;
    margin: auto;
    ul {
      padding: 0;
      li {
        height: 40px;
        font-size: 25px;
        border-bottom: 1px solid #eaffd0;
        margin-bottom: 10px;
        display: flex;
        .todo-name {
          width: calc(100% - 200px);
        }
        .might-done {
          text-decoration: line-through wavy #24bffb;
        }
        .spacer {
          flex-grow: 1;
        }
        .todo-action {
          height: 100%;
          padding: 6px 0;
          img {
            width: 24px;
            cursor: pointer;
            margin-left: 10px;
            &:active {
              border-radius: 50%;
              border: 1px solid #95e1d3;
            }
          }
        }
      }
    }
  }
}
</style>
