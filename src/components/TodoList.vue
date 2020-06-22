<template>
    <div>
        Todo List goes here
        <div class="todo-app">
            <input type="text" class="todo-input" name="" v-model="todoItem" @keyup.enter="addTodo" >
            <ul>
                <li v-for="(todo, i) in todoList" :key="i">
                    <input type="checkbox" name="todo[]" v-bind:id="'todoItem_' + todo.id">
                    <label v-bind:for="'todoItem_' + todo.id">{{ todo.title }}</label>
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
        }
    }
}
</script>

<style lang="postcss">
.todo-app {
    text-align: left;
}
input[type=text], .form-control {
    width: 100%;
    padding: 10px 15px;
    border: 1px solid lightblue;
    lineheight: 17px;
}
.todo-input {
    margin: 20px auto;
}
.todo-app ul li {
    margin: 10px auto;
}
.todo-app input[type=checkbox] {
    display: inline-block;
    margin-right: 10px;
}
</style>