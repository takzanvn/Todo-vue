<template>
    <div>
        Todo List goes here
        <div class="todo-app">
            <input type="text" class="todo-input" name="" v-model="todoItem" @keyup.enter="addTodo" placeholder="What needs to be done">
            <ul>
                <li v-for="(todo, i) in todoList" :key="i">
                    <input type="checkbox" name="todo[]" v-bind:id="'todoItem_' + todo.id" v-model="todo.completed">
                    <div class="todo-title-wrapper">
                        <label v-bind:for="'todoItem_' + todo.id" v-if="!todo.editing" @dblclick="editTodo(todo)" :class="{ checked: todo.completed }">{{ todo.title }}</label>
                        <input type="text" class="form-control" v-model="todo.title" v-else @blur="doneEdit(todo)" v-focus 
                        @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)">
                    </div>
                    <span class="close-button" @click="removeTodo(i)">&times;</span>
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
                id: 0,
                editing: false,
            }],
            beforeEditCache: ''
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
                id: this.todoList.length,
                editing: false,
            })
            this.todoItem = ""
        },
        removeTodo(index) {
            this.todoList.splice(index, 1)
        },
        editTodo(el) {
            this.beforeEditCache = el.title
            el.editing = true
        },
        doneEdit(el) {
            if (el.title.trim() == '') {
                el.title = this.beforeEditCache
            }
            el.editing = false
        },
        cancelEdit(el) {
            el.title = this.beforeEditCache
            el.editing = false
        }
    },
    directives: {
        focus: {
            inserted: function(el) {
                el.focus()
            }
        }
    }
}
</script>

<style lang="scss" scoped>
$input_fontsize: 18px;

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
    font-size: $input_fontsize;
}
.todo-app {
    ul li {
        margin: 3px auto 0;
        min-height: 45px;
    }
    li {
        display: flex;
        align-items: center;
    }
    input[type=checkbox] {
        display: inline-block;
        margin-right: 10px;
    }
    .todo-title-wrapper {
        flex-grow: 1;
        .form-control {
            font-size: $input_fontsize;
        }
        label {
            display: block;
            &.checked {
                text-decoration: line-through;
                color: grey;
            }
        }
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