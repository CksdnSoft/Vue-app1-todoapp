<template>
    <div>
        <todo-item
            v-for="todo in todos"
            :key="todo.id"
            :todo="todo"
            @update-todo="updateTodo"
            @delete-todo="deleteTodo"
        />
        <hr/>
        <todo-creator @create-todo="createTodo"/>
    </div>
</template>

<script>
import lowdb from 'lowdb'
import LocalStorage from 'lowdb/adapters/LocalStorage'
import cryptoRandomString from 'crypto-random-string'
import _cloneDeep from 'lodash/cloneDeep'
import _find from 'lodash/find'
import _assign from 'lodash/assign'
import _findIndex from 'lodash/findIndex'
import TodoCreator from './TodoCreator'
import TodoItem from './TodoItem'

export default {
    components: {
        TodoCreator,
        TodoItem
    },
    data() {
        return {
            db: null,
            todos: []
        }
    },
    created() {
        this.initDB()
    },
    methods: {
        initDB() {
            const adapter = new LocalStorage('todo-app')
            this.db = lowdb(adapter)

            const hasTodos = this.db.has('todos').value()

            if(hasTodos) {
                this.todos = _cloneDeep(this.db.getState().todos)
                console.log(this.todos)
            } else {
                this.db
                .defaults({
                    todos: [],
                })
                .write()
            }  
        },
        createTodo(title) {
            const newTodo = {
                id: cryptoRandomString({ length: 10 }),
                title,
                createdAt: new Date(),
                updatedAt: new Date(),
                done: false
            }
            
            console.log(this.todos)
            this.db
                .get('todos')
                .push(newTodo)
                .write()
            
            console.log(this.todos)
            this.todos.push(newTodo)
            console.log(this.todos)
        },
        updateTodo(todo, value) {
            this.db
                .get('todos')
                .find({id: todo.id})
                .assign(value)
                .write()

            const foundTodo = _find(this.todos, {id: todo.id})
            _assign(foundTodo, value)
        },
        deleteTodo(todo) {
            this.db
                .get('todos')
                .remove({id: todo.id})
                .write()
            
            const foundIndex = _findIndex(this.todos, {id: todo.id})
            this.$delete(this.todos, foundIndex)
        }
    }
}
</script>

<style>

</style>