<template>
    <div>
        <button @click="createTodo">Insert</button>
        <input
            :value="title"
            :placeholder="placeholder"
            type="text"
            @input="title = $event.target.value"
            @keypress.enter="createTodo"
        />
    </div>
</template>

<script>
export default {
    data() {
        return {
            title: '',
            placeholder: "Insert To-do"
        }
    },
    methods: {
        createTodo() {
            const validatedTitle = this.title && this.title.trim()
            if(!validatedTitle) {
                alert('Not suitable to insert')
                this.title = this.title.trim()
                return
            }

            this.$emit('create-todo', this.title)
            this.title = ''
        }
    }
}
</script>

<style>

</style>