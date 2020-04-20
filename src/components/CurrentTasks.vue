<template>
    <div>
        <h1 class="text-center mb-3"><u>Current Tasks</u></h1>
        <div class="todos">
            <b-list-group v-for="todo in this.todo" class="col-10 my-1 mx-auto"  :class="{editing: todo.edit}"  :key="todo.id">
                <b-card>
                    <div v-if="todo.edit">
                        <input v-model="editingTodo" class="col" @keydown.enter="finalizeTodo(todo, editingTodo)" :placeholder="todo.label">
                        <img class="m-2" @click="finalizeTodo(todo, editingTodo)" src='../assets/checked.svg' height="45" width="35">
                        <img class="m-2" @click="todo.edit = false, editingTodo = ''" src="../assets/cancel.svg" height="45" width="35">
                    </div>

                    <b-card-text style="color: black" class="ml-3" v-else>
                        {{todo.label}}
                    </b-card-text>

                    <b-card-footer class="text-right">
                        <img class="mx-2 listItem" @click="completeTodo(todo)" src="../assets/tick.svg" height="25" width="25">
                        <img class="mx-2 listItem" @click="editTodo(todo)" src="../assets/more.svg" height="25" width="25">
                        <img class="mx-2 listItem" @click="removeTodo(todo)" src="../assets/garbage-2.svg" height="25" width="25">

                    </b-card-footer>
                </b-card>
            </b-list-group>
            <h1 class="display3 text-center" v-if="!this.todo.length && this.complete.length">No tasks here! Good job!
                <small style="font-size: small"><br>maybe add some more though.</small></h1>
        </div>
    </div>
</template>

<script>
    export default {
        name: "CurrentTasks",
        props: ['todo','complete'],
        data() {
            return {
                editingTodo: '',
            }
        },
        methods: {
            removeTodo(todo) {
                const index = this.todo.indexOf(todo);
                this.todo.splice(index, 1);
                localStorage.setItem('toDoLists', JSON.stringify(this.todo))
            },
            finalizeTodo(todo, e) {
                const index = this.todo.indexOf(todo);
                if (e === '') {
                    todo.edit = false
                    return;
                }
                this.todo[index].label = e
                todo.edit = false
                localStorage.setItem('toDoLists', JSON.stringify(this.todo))
            },
            editTodo(todo) {
                this.todo.forEach((e) => {
                    e.edit = false
                })
                todo.edit = !todo.edit
            },
            completeTodo(todos) {
                this.todo.forEach((e) => {
                    e.edit = false
                })
                const index = this.todo.indexOf(todos);
                this.complete.push(todos)
                this.todo.splice(index, 1);
                localStorage.setItem('toDoLists', JSON.stringify(this.todo))
                localStorage.setItem('completedTodos', JSON.stringify(this.complete))
            }
        }
    };
</script>

<style scoped>
    .listItem {
        cursor: pointer;
    }
</style>
