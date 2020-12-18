<template>
    <div class='todo-item'>
        <div
            v-if="isEditMode"
            class="item__inner item--edit"
        >
            <input
                ref="titleInput"
                :value="editedTitle"
                type="text"
                @input="editedTitle = $event.target.value"
                @keypress.enter="editedTodo"
                @keypress.esc="offEditMode"
            />
            <div class="item__actions">
                <button 
                    key="done"
                    @click="editedTodo">Done</button>
                <button
                    key="cancel"
                    @click="offEditMode">Cancel</button>
            </div>
        </div>
        <div 
            v-else
            class="item__inner item--normal"
        >
            <input
                v-model="done"
                type="checkbox"
            />
            <div class="item__title-wrap">
                <div class="item__title">
                    {{todo.title}}
                </div>
                <div class="item__date">
                    {{ date }}
                </div>
            </div>
            <div class="item__actions">
                <button
                    key="update"
                    @click="onEditMode">Edit</button>
                <button 
                    key="delete"
                    @click="deleteTodo">Delete</button>
            </div>
        </div>
    </div>
</template>

<script>
import dayjs from 'dayjs'

export default {
    props: {
        todo: Object
    },
    data() {
        return {
            isEditMode: false,
            editedTitle: false,
        }
    },
    computed: {
        done: {
            get() {
                return this.todo.done
            },
            set(done) {
                this.updateTodo({
                    done: done
                })
            }
        },
        date() {
            const date = dayjs(this.todo.createdAt)
            const isSame = date.isSame(this.todo.updateAt)
            if(isSame) {
                return date.format('YYYY/MM/DD')
            } else {
                return `${date.format('YYYY/MM/DD')} (edited)`
            }
        }
    },
    methods: {
        editedTodo() {
            if(this.todo.title !== this.editedTitle) {
                this.updateTodo({
                    title: this.editedTitle,
                    updatedAt: new Date()
                })
            }

            this.offEditMode()
        },
        onEditMode() {
            this.isEditMode = true
            this.editedTitle = this.todo.title

            this.$nextTick(()=>{
                this.$refs.titleInput.focus()
            })
        },
        offEditMode() {
            this.isEditMode = false
        },
        updateTodo(value) {
            this.$emit('update-todo', this.todo, value)
        },
        deleteTodo() {
            this.$emit('delete-todo', this.todo)
        }
    }
}
</script>

<style scoped lang="scss">
.todo-item {
    margin-bottom: 10px;
    .item__inner {
        display: flex;
    }
    .item__date {
        font-size:12px;
    }
    &.done {
        .item__title {
            text-decoration: line-through
        }
    }
}

</style>