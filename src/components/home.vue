<template>
    <div class="todo-list-section">
        <div class="container">

            <div class="row">
                <div class="col-md-6 offset-3">

                    <div class="title-area">
                        <h4>All Todo List</h4>
                    </div>
                    <div class="add-new-todo-area">
                        <input type="text" @keyup.enter="addNewTodo" v-model="newTodo"
                            class=" form-control form-control-sm" placeholder="Add New Todo">
                    </div>
                    <div class="todo-list-area">
                        <div v-for="(todo, index) in todos" :key="todo.id" class="single-todo">
                            <div class="row">
                                <div class="col-md-2">
                                    <input @change="completeTodo(todo)" class="todo-checkbox" type="checkbox">
                                </div>
                                <div v-if="todo.completed == true" class="col-md-8">
                                    <strike><span> {{ todo.todo_name }} </span></strike>
                                </div>
                                <div v-else class="col-md-8">
                                    <div v-if="todo.edit == false" @dblclick="editToInput(todo)" class="todo-label">
                                        <span> {{ todo.todo_name }} </span>
                                    </div>
                                    <div v-else class="edit-todo-input-area">
                                        <input @keyup.esc="cancelEdit(todo)" v-focus @blur="doneEdit(todo)"
                                            @keyup.enter="doneEdit(todo)" class="edit-todo-input" type="text"
                                            v-model="todo.todo_name">
                                    </div>
                                </div>
                                <div class="col-md-2">
                                    <a @click.prevent="removeTodo(index)" class="todo-remove-button " href=""> &times;
                                    </a>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="todo-footer-area">
                        <div class="row">
                            <div class="col-md-6">
                                <div class="check-all-section text-left">
                                    <div class="checked-checkbox">
                                        <input  :checked="!completeCountTrue" type="checkbox"> Check all
                                    </div>                                   
                                </div>
                            </div>
                            <div class="col-md-6">
                                <div class="left-item-count-area text-right">
                                    <p>Left Items : {{countUnCompletedTodos}}</p>
                                </div>
                            </div>
                        </div>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                newTodo: '',
                todoId: 3,
                beforeTodoNameCache: '',
                todos: [
                    {
                        id: 1,
                        todo_name: 'This Is Todo 1',
                        edit: false,
                        completed: false
                    },
                    {
                        id: 2,
                        todo_name: 'This is todo 2',
                        edit: false,
                        completed: false
                    },
                ],

            }
        },
        computed: {
            countUnCompletedTodos(){
               return this.todos.filter(todo => todo.completed == false).length
            },
            completeCountTrue(){
                return this.countUnCompletedTodos !== 0
            }
        },
        methods: {
            addNewTodo() {
                if (this.newTodo.length == 0) {
                    return
                }
                this.todos.push({
                    id: this.todoId,
                    todo_name: this.newTodo,
                    edit: false,
                    completed: false
                })
                this.newTodo = ""
                this.todoId++
            },
            removeTodo(index) {
                this.todos.splice(index, 1)
            },
            editToInput(todo) {
                this.beforeTodoNameCache = todo.todo_name
                todo.edit = true
            },
            doneEdit(todo) {
                if (todo.todo_name.length == 0) {
                    todo.todo_name = this.beforeTodoNameCache
                }
                todo.edit = false

            },
            cancelEdit(todo) {
                todo.todo_name = this.beforeTodoNameCache
                todo.edit = false
            },
            completeTodo(todo) {
                if (todo.completed == false) {
                    return todo.completed = true
                } else {
                    return todo.completed = false
                }
            }
            
        },
        directives: {
            focus: {
                // directive definition
                inserted: function (el) {
                    el.focus()
                }
            }
        }
    }
</script>
<style scoped>
    .todo-list-section {
        margin-top: 50px;

        font-size: 16px;
    }
    .todo-checkbox {
        height: 10px;
        width: 10px;
        margin-top: 7px;
        margin-right: 15px;
        margin-left: 10px;
        background: lightblue;
    }
    .todo-list-area {
        /* background: rgb(190, 218, 245); */
        padding: 15px 10px;
        border-radius: 4px;
    }
    .add-new-todo-area {
        margin-bottom: 5px;
    }
    span {
        font-family: sans-serif;
        color: darkslategray;
        font-size: 18px;
    }

    .title-area {
        color: darkcyan;
    }
    .single-todo {
        margin-top: 10px;
    }
    a.todo-remove-button {
        color: black;
        text-decoration: none;
    }
    .edit-todo-input {
        height: 25px;
        border-radius: 2px;
        border: 1px solid gray;
        padding-left: 5px;
        outline: none;
        background-color: rgb(241, 241, 241);
    }
    .todo-footer-area{
        border-top:1px solid lightgray;
    }
    .left-item-count-area{
        padding-right: 25px;
    }
    .check-all-section{
        padding-left: 25px;
    }
    .todo-checkbox{
        cursor: pointer;
    }
</style>