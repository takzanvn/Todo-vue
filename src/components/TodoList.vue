<template>
    <div>
        Todo List goes here
        <div class="todo-app">
            <input type="text" class="todo-input" name="" v-model="todoItem" @keyup.enter="addTodo" placeholder="What needs to be done">
            <ul>
                <li v-for="(todo, index) in todoList" :key="index">
                    <input type="checkbox" name="todo[]" v-bind:id="'todoItem_' + todo.id">
                    <label v-bind:for="'todoItem_' + todo.id">{{ todo.title }}</label>
                    <span class="close-button" @click="removeTodo(index)">&times;</span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodoList',
    data() {
        return {
            todoItem: "",
            todoList: [{
                title: "Finish toto list by Vuejs",
                completed: false,
                id: 0
            }]
        }
    },
    methods: {
        addTodo() {
            if (this.todoItem.trim().length == 0) {
                return
            }
            this.todoList.push({
                title: this.todoItem,
                completed: false,
                id: this.todoList.length
            })
            this.todoItem = ""
        },
        removeTodo(index) {
            this.todoList.splice(index, 1)
        }
    }
}
</script>

<style lang="scss" scoped>
.todo-app {
    text-align: left;
}
input[type=text], .form-control {
    width: 100%;
    padding: 10px 15px;
    border: 1px solid #41b783;
    line-height: 17px;
}
.todo-input {
    margin: 20px auto;
    font-size: 18px;
}
.todo-app {
    ul li {
        margin: 10px auto;
    }
    input[type=checkbox] {
        display: inline-block;
        margin-right: 10px;
    }
    .close-button {
        float: right;
        display: block;
        width: 20px;
        height: 20px;
        font-size: 22px;
        text-align: center;
        color: #AAA;
        cursor: pointer;
        &:hover {
            color: #222;
        }
    }
}
</style>