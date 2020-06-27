<template>
    <div>
        Todo List goes here
        <div class="todo-app">
            <input class="form-control todo-input" type="text" name="" v-model="todoItem" @keyup.enter="addTodo" placeholder="What needs to be done">
            <div class="todo-extra">
                <input type="checkbox" id="check-all" class="check-all" @change="checkAll()" :checked="checkAllFlag">
                <label for="check-all">Check All</label>
                <span class="todo-remaining float-r">{{ remaining }} item<span v-show="remaining">s</span> left</span>
                <div class="todo-sub-extra">
                    <button class="btn-filter" :class="{active: this.filtered == 'all'}" @click="filter('all')">All</button>
                    <button class="btn-filter" :class="{active: this.filtered == 'active'}" @click="filter('active')">Active</button>
                    <button class="btn-filter" :class="{active: this.filtered == 'completed'}" @click="filter('completed')">Completed</button>
                    <button class="btn-filter float-r" @click="clearCompleted()" v-show="countChecked > 0">Clear completed</button>
                </div>
            </div>
            <transition-group enter-active-class="fade-in-up" leave-active-class="fade-out-down">
                <div class="todo-item" v-for="(todo, i) in todoFiltered" :key="i + 0">
                    <input type="checkbox" name="todo[]" v-bind:id="'todoItem_' + todo.id" v-model="todo.completed" @change="refreshFilter()">
                    <div class="todo-title-wrapper">
                        <label v-bind:for="'todoItem_' + todo.id" v-if="!todo.editing" @dblclick="editTodo(todo)" :class="{ checked: todo.completed }">{{ todo.title }}</label>
                        <input type="text" class="form-control" v-model="todo.title" v-else @blur="doneEdit(todo)" v-focus 
                        @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)">
                    </div>
                    <span class="close-button" @click="removeTodo(i)">&times;</span>
                </div>
            </transition-group>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodoList',
    data() {
        return {
            todoList: [{
                title: "Finish toto list by Vuejs",
                completed: false,
                id: 0,
                editing: false,
            }],
            beforeEditCache: '',
            filtered: 'all'
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
        },
        checkAll() {
            this.todoList.forEach(todo => todo.completed = event.target.checked)
        },
        filter(input) {
            return this.filtered = input
        },
        refreshFilter() {
            return this.todoFiltered
        },
        clearCompleted() {
            return this.todoList = this.todoList.filter(todo => !todo.completed)
        }
    },
    computed: {
        remaining() {
            return this.todoList.filter(todo => !todo.completed).length
        },
        countChecked() {
            return this.todoList.length - this.remaining
        },
        checkAllFlag() {
            if (this.todoList.length == 0) return false
            if (this.remaining == 0) return true
        },
        todoFiltered() {
            if (this.filtered == 'active') {
                return this.todoList.filter(todo => !todo.completed)
            } else if (this.filtered == 'completed') {
                return this.todoList.filter(todo => todo.completed)
            } else {
                return this.todoList
            }
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
    label {
        cursor: pointer;
    }
}
.form-control {
    width: 100%;
    padding: 10px 15px;
    border: 1px solid #666;
    line-height: 17px;
    &:focus {
        border-color: #41b783
    }
}
.todo-input {
    margin: 20px auto;
    font-size: $input_fontsize;
}
.todo-extra {
    border-bottom: 1px solid #DDD;
    padding-bottom: 10px;
}
.todo-sub-extra {
    margin-top: 10px;
}
.todo-app {
    .todo-item {
        margin: 3px auto 0;
        min-height: 45px;
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
button[class^=btn]{
    padding: 3px 6px;
    background-color: #DDD;
    border-radius: 3px;
    cursor: pointer;
    &.active {
        background: lightgreen;
    }
}

@keyframes todoItem {
    0% {
        opacity: 0;
        transform: translateY(30px);
    }
    100% {
        opacity: 1;
        transform: translateY(0);
    }
}
.fade-in-up {
    animation: todoItem .4s ease-out forwards;
}
.fade-out-down {
    animation: todoItem .4s ease-in reverse;
}
</style>