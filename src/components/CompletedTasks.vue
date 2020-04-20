<template>
    <div>
        <div class="todos">
            <h1 class="text-center mb-3"><u>Completed</u></h1>
            <b-list-group v-for="todos in this.complete" class="col-10 my-1 mx-auto"  :class="{editing: todos.edit}"  :key="todos.id">
                <b-card>
                    <div v-if="todos.edit">
                        <input v-model="editCompTodo" class="col" @keydown.enter="finalizeTodo(todos, editCompTodo)" :placeholder="todos.label">
                        <img class="m-2" @click="finalizeTodo(todos, editCompTodo)" src='../assets/checked.svg' height="45" width="35">
                        <img class="m-2" @click="todos.edit = false, editCompTodo = ''" src="../assets/cancel.svg" height="45" width="35">
                    </div>

                    <b-card-text v-else style="color: black" class="ml-3">
                        {{todos.label}}
                    </b-card-text>
                    <b-card-footer class="text-right">
                        <img class="mx-2 listItem" @click="todos.edit = !todos.edit" src="../assets/more.svg" height="25" width="25">
                        <img class="mx-2 listItem" @click="removeTodo(todo)" src="../assets/garbage-2.svg" height="25" width="25">
                    </b-card-footer>
                </b-card>
            </b-list-group>
        </div>
    </div>
</template>

<script>
    export default {
        name: "CompletedTasks",
        props: ['todo', 'complete'],
        data() {
            return {
                editCompTodo: '',
            }
        },
        methods: {
            removeTodo(todo) {
                const index = this.complete.indexOf(todo);
                this.complete.splice(index, 1);
                localStorage.setItem('completedTodos', JSON.stringify(this.complete))
            },
            finalizeTodo(todo, e) {
                const index = this.complete.indexOf(todo);
                if (e === '') {
                    todo.edit = false
                    return;
                }
                this.complete[index].label = e
                todo.edit = false
                localStorage.setItem('completedTodos', JSON.stringify(this.complete))
            },
        }
    }
</script>

<style scoped>

</style>
