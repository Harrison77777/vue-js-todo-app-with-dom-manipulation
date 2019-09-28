<template>
  <div class="todo-list-section">
    <div class="container">
      <div class="row">
        <div class="col-md-6 offset-3">
          <div class="title-area">
            <h4>All Todo List</h4>
          </div>
          <div class="add-new-todo-area">
            <input type="text" @keyup.enter="addNewTodo" v-model="newTodo" class="form-control form-control-sm"
              placeholder="Add New Todo" />
          </div>
          <div class="todo-list-area">
            <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="single-todo">
              <div class="row">
                <div class="col-md-2">
                  <input v-model="todo.completed" :checked="todo.complete == true" class="todo-checkbox"
                    type="checkbox" />
                </div>

                <div class="col-md-8">
                  <div v-if="todo.edit == false" @dblclick="editToInput(todo)" :class="{complete : todo.completed}"
                    class="todo-label">
                    <span>{{ todo.todo_name }}</span>
                  </div>
                  <div v-else class="edit-todo-input-area">
                    <input @keyup.esc="cancelEdit(todo)" v-focus @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)"
                      class="edit-todo-input" type="text" v-model="todo.todo_name" />
                  </div>
                </div>
                <div class="col-md-2">
                  <a @click.prevent="removeTodo(index)" class="todo-remove-button" href>&times;</a>
                </div>
              </div>
            </div>
          </div>
          <div class="todo-footer-area">
            <div class="row">
              <div class="col-md-6">
                <div class="check-all-section text-left">
                  <div class="checked-checkbox text-left">
                    <input @change="checkAllTodos" :checked="!completeCountTrue" type="checkbox" /> Check all
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
          <div class="todo-filtering-options">
            <div class="row">
              <div class="col-md-7">
                <div class="row">
                  <div class="option-buttons">
                    <button class="" :class="{ active : filter == 'all' }" @click="filter = 'all'">All</button>
                    <button class="" :class="{ active : filter == 'active' }" @click="filter = 'active'">Active</button>
                    <button class="" :class="{ active : filter == 'completed' }" @click="filter = 'completed'">Completed</button>
                  </div>
                </div>
              </div>
              <div class="col-md-5">
                <div v-if="countCompletedTodos > 0" class="clean-all-completed-button text-right">
                    <a @click.prevent="cleanAllCompletedTodos" href="#"> Clean All Completed Todos</a>
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
        newTodo: "",
        todoId: 3,
        filter : 'all',
        beforeTodoNameCache: "",
        todos: [
          {
            id: 1,
            todo_name: "This Is Todo 1",
            edit: false,
            completed: false
          },
          {
            id: 2,
            todo_name: "This is todo 2",
            edit: false,
            completed: false
          }
        ]
      };
    },


    computed: {
      countUnCompletedTodos() {
        return this.todos.filter(todo => todo.completed == false).length;
      },
      countCompletedTodos(){
         let completedTodo = 0
         this.todos.filter(todo => {
          completedTodo += todo.completed == true
        })
        return completedTodo;
        
      },
      completeCountTrue() {
        return this.countUnCompletedTodos !== 0;
      },
      todosFiltered(){
        if (this.filter == 'all') {

            return this.todos

        }else if(this.filter == 'active'){

          return this.todos.filter(todo => !todo.completed)

        }else if(this.filter == 'completed'){

          return this.todos.filter(todo =>  todo.completed)

        }
        return this.todos
      }

    },



    methods: {
      addNewTodo() {
        if (this.newTodo.length == 0) {
          return;
        }
        this.todos.push({
          id: this.todoId,
          todo_name: this.newTodo,
          edit: false,
          completed: false
        });
        this.newTodo = "";
        this.todoId++;
      },
      removeTodo(index) {
        this.todos.splice(index, 1);
      },
      editToInput(todo) {
        this.beforeTodoNameCache = todo.todo_name;
        todo.edit = true;
      },
      doneEdit(todo) {
        if (todo.todo_name.length == 0) {
          todo.todo_name = this.beforeTodoNameCache;
        }
        todo.edit = false;
      },
      cancelEdit(todo) {
        todo.todo_name = this.beforeTodoNameCache;
        todo.edit = false;
      },
      completeTodo(todo) {
        if (todo.completed == false) {
          return (todo.completed = true);
        } else {
          return (todo.completed = false);
        }
      },
      checkAllTodos() {
        this.todos.forEach(todo => {
          return todo.completed = event.target.checked
        })
      },
      cleanAllCompletedTodos(){
        return this.todos = this.todos.filter(todo => {
          return todo.completed == false
        })
      }
      
    },


    
    directives: {
      focus: {
        // directive definition
        inserted: function (el) {
          el.focus();
        }
      }
    }
  };
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
    margin-left: 0px;
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

  .todo-footer-area {
    border-top: 1px solid lightgray;
    border-bottom: 1px solid lightgray;
    padding-top: 15px;
  }

  .left-item-count-area {
    padding-right: 25px;
  }

  .check-all-section {
    padding-left: 9px;
  }

  .todo-checkbox {
    cursor: pointer;
  }

  .complete {
    text-decoration: line-through;
  }

  .todo-filtering-options button {
    height: 30px;
    width: 90px;
    outline: none;
    margin-right: 1px;
    text-align: center;
    background: skyblue;
    border: none;
    border-radius: 5px;
    color: ghostwhite;
  }

  .todo-filtering-options {
    padding-left: 20px;
    margin-top: 6px;

  }

  .todo-filtering-options a {
    color: black;
    text-decoration: none;

  }

  .active {
    background: rgb(139, 202, 139) !important;
  }
</style>