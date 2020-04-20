<template>
    <div>
    <h1 class="display-3 mt-5 text-center">To-do</h1>
        <b-form-input class="mx-auto col my-5" v-model="currentTodo" v-on:keydown.enter="$emit('switchViewer')" @keydown.enter="addTodo" placeholder="Enter a task!"></b-form-input>
    </div>
</template>

<script>
    export default {
        name: "TaskAdder",
        props: ['todo','window'],
        data() {
            return {
                currentTodo: ''
            }
        },
        methods: {

            addTodo() {
                this.todo.push({id: this.todo.length, label: this.currentTodo, edit: false, completed: false});
                localStorage.setItem('toDoLists', JSON.stringify(this.todo))
                this.currentTodo = '';

            },
        },
    }
</script>

<style scoped>

</style>
