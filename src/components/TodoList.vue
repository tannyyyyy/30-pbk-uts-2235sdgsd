<template>
  <div style="background-color:cornsilk;">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css">
    <div>To Do list - Keranjang belanjaan</div>
    <input style="color: black;" type="text" class="todo-input" placeholder="Nama Barang"
    v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp faster" leave-active-class="animated fadeOutDown faster">
    <todo-item v-for="(todo, index) in todosFiltered" 
    :key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining"
    @removedTodo="removeTodo" @finishedEdit="finishedEdit">
        <!-- <div class="todo-item-left">
            
            <input type="checkbox" v-model="todo.completed">
            
            <div v-if="!todo.editing" @dblclick="editTodo(todo)" 
            class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>
            
            <input v-else class="todo-item-edit" type="text" 
            v-model="todo.title" @blur="doneEdit(todo)" 
            @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)"
            v-focus>
            
        </div>
        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div> -->
    </todo-item>
    </transition-group>
    <div class="extra-container">
        <div><label><input type="checkbox" :checked="anyRemaining" 
        @change="checkAllTodos"> Cek Semua </label></div>
        <div>{{ remaining }} Barang tersisa</div>
    </div>

    <div class="extra-container">
        <div>
            <button :class="{ active: filter == 'all' }" @click="filter = 'all'">semua</button>
            <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Belum terbeli</button>
            <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Sudah dibeli</button>
        </div>

        <div> 
            <button v-if="showClearCompletedButton" @click="clearCompleted">Hapus yang sudah</button>
        </div>

    </div>
  </div>
</template>

<script>
import TodoItem from './TodoItem'
export default {
  name: 'TodoList',
  components: {
    TodoItem,
  },
  data () {
    return {
        newTodo: '',
        idForTodo: 4,
        beforeEditCache: '',
        filter: 'all',
        todos: [
            {
                'id': 1,
                'title': 'Laptop',
                'completed': false,
                'editing': false,
            },
            {
                'id': 2,
                'title': 'Charger',
                'completed': false,
                'editing': false,
            },
            {
                'id': 3,
                'title': 'Alat tulis',
                'completed': false,
                'editing': false,
            },
        ]
    }
    },
    
    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.completed).length
        },
        anyRemaining() {
            return this.remaining == 0
        },
        todosFiltered() {
            if (this.filter == 'all') {
                return this.todos
            } else if (this.filter == 'active') {
                return this.todos.filter(todo => !todo.completed)
            } else if (this.filter == 'completed') {
                return this.todos.filter(todo => todo.completed)
            }
        },
        showClearCompletedButton() {
            return this.todos.filter(todo => todo.completed).length > 0
        }
    },
    methods: {
        addTodo() {
            if (this.newTodo.trim().length == 0) {
                return
            }
            this.todos.push({
                id: this.idForTodo,
                title: this.newTodo,
                completed: false,
            })

            this.newTodo = ''
            this.idForTodo++
        },
        editTodo(todo) {
            this.beforeEditCache = todo.title
            todo.editing = true
        },
        doneEdit(todo) {
            if (this.title.trim().length == 0) {
                todo.title = this.beforeEditCache
            }
            todo.editing = false
        },
        cancelEdit(todo) {
            todo.title = this.beforeEditCache
            todo.editing = false
        },
        removeTodo(index) {
            this.todos.splice(index, 1)
        },
        checkAllTodos() {
            this.todos.forEach((todo) => todo.completed = event.target.checked)
        },
        clearCompleted() {
            this.todos = this.todos.filter(todo => !todo.completed  )
        },
        finishedEdit(data) {
            this.todos.splice(data.index, 1, data.todo)
        }
    },
        
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .todo-input {
        color:black;
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
    }
    .todo-item {
        color: black;
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 1s;
    }
    .remove-item {
        cursor: pointer;
        margin-left: 14px;
    }
    .todo-item-left {
        display: flex;
        align-items: center;
    }
    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }
    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Arial, Helvetica, sans-serif;
    }
    .completed {
        text-decoration: line-through;
        color: gray;
    }
    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgray;
        padding-top: 14px;
        margin-bottom: 14px;
    }
    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
    }
    
    .active {
        background: lightgreen;
    }

</style>
