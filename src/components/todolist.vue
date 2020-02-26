<template>
    <div class="cont">
        <input type="text" name="todo-field" id="todo__input" class="todo__input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter='addTodo' >

        <!-- <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown"> -->
        <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
          <div class="main-item todo-item-left">
            <input type="checkbox" v-model="todo.completed">
            <div class="editable todo-item-label" v-if="!todo.editing" @dblclick="editTodo(todo)" :class="{completed : todo.completed}">
              {{todo.title}}
            </div>
              <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus @keyup.esc="cancelEdit(todo)">
          </div>

          <div class="remove-item" @click="removeTodo(index)">
            &times;
          </div>
        </div>
        <!-- </transition-group> -->

          <div class="extra-container">
            <div class=""><label><input type="checkbox"  :checked="!anyRemaining" @change="checkAllTodos">Check All</label></div>
            <div>{{ remaining }} items left</div>
          </div>

          <div class="extra-container">
            <div>
              <button :class="{active: filter == 'all'}" @click="filter = 'all' "> All</button>
              <button :class="{active: filter == 'active'}" @click="filter = 'active'"> Active</button>
              <button :class="{active: filter == 'completed'}" @click="filter = 'completed'"> Completed </button>
            </div>

            <div>
              <transition name="fade">
                <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
              </transition>
            </div>

          </div>


    </div>
</template>

<script>
export default {
  name: 'todolist',
  data () {
    return {
      newTodo: '',
      NewAuthor:'arjan',
      idForTOdo: 3,
      beforeEditCache:'',
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': 'Finish my work',
          'compleated':false,
          'author': "Arjan",
          'editing':false,
        },
        {
          'id': 2,
          'title': 'Finish watching Todo App',
          'compleated':false,
          'author': "Arjan Joaquin",
          'editing':false,

        },
         {
          'id': 3,
          'title': 'You  are @ 36:49 ',
          'compleated':false,
          'author': "Arjan Joaquin",
          'editing':false,

        }
      ]
    }
  },
  computed: {
    remaining(){
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining(){
      return this.remaining != 0
    },
    todosFiltered(){
      if(this.filter == 'all'){
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
        } else if (this.filter == 'completed') {
          return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
    showClearCompletedButton(){
        return this.todos.filter(todo => todo.completed).length > 0
    },
  },
  directives:{
    focus:{
      inserted: function(el){
        el.focus()
      }
    }
  },
  methods:{
    addTodo(){
      if(this.newTodo.trim().length == 0){
        alert("Input Empty")
        return
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed:false,
        author: this.NewAuthor,
        edit:false,
        
      }) 

      this.newTodo = ''
      this.idForTodo++
    },
    editTodo(todo){
      this.beforeEditCache = todo.title
      todo.editing = true
    },
    removeTodo(index){
      this.todos.splice(index ,1)
    },
    doneEdit(todo){
      if(todo.title.trim() == ''){
        todo.title = this.beforeEditCache
      }
      todo.editing = false
    },
    cancelEdit(todo) {
      todo.editing = false
      todo.title = this.beforeEditCache
    },
    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted(){
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<style>
    /* @import url(https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.css); */

    .todo__input{
        width: 100%;
        padding: 10px 10px;
        font-size: 18px;
        margin-bottom: 30px;
    }

    .todo-item{
      margin-bottom:  12px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      animation-duration: 0.3s;
      /* padding: 10px 5px;
      border: 1px solid rgba(116, 116, 116, 0.691);
      background: #ffffff;
      box-shadow:  32px 32px 56px #b8b8b8, 
             -32px -32px 56px #ffffff; */
    }
    .remove-item{
      cursor: pointer;
      font-size: 19px;
    }
    .remove-item:hover{
      font-size: 21px;
    }
    img {
      border-radius: 50%;
      -webkit-transition: -webkit-transform .8s ease-in-out;
              transition:         transform .8s ease-in-out;
      }
    img:hover {
      -webkit-transform: rotate(360deg);
              transform: rotate(360deg);
    }
    .main-item{
      font-size: 23px;
      text-transform: capitalize;
    }

    .todo-item-left{
      display: flex;
      align-items: center;
    }

    .todo-item-label{
      padding: 18px;
      border: 1px solid white;
      margin-left: 12px;
    }

    .todo-item-edit{
      font-size: 23px;
      color: #2c3e50;
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      font-family: Arial, Helvetica, sans-serif;
    }

    .completed{
      text-decoration: line-through;
      color: gray;
    }

    .extra-container{
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 16px;
      border-top: 1px solid lightgray;
      padding-top: 14px;
      margin-bottom: 14px;
    }

    button{
      font-size: 14px;
      background-color: white;
      appearance: none;
    }

    button:hover{
      background: lightgreen;
    }

    button:focus{
      outline: none;
    }

    .active{
      background: lightgreen
    }

    .fade-enter-active{
      transition: opacity .2s;
    }
    .fade-leave-active{
      transition: opacity .2s;
    }
    .fade-enter{
      opacity: 0;
    }
    .fade-leave-to{
     opacity: 0;
    }


</style>

