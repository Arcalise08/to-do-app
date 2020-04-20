<template>
    <div class="container">
        <div class="row">
            <div class="col">
             <task-adder v-bind:todo="todos" v-on:switchViewer="switchViews('current', true)"/>
                <div class="text-center mb-3">
                    <button id="current" @click="switchViews($event)" :class="{active: this.activeWin === 'current'}" class="btn mx-1 btn-outline-light">Current Tasks</button>
                    <button id="completed" @click="switchViews($event)" :class="{active: this.activeWin === 'completed'}" class="btn mx-1 btn-outline-light">Completed Tasks</button>
                </div>
             <current-tasks v-if="this.activeWin === 'current'" v-bind:complete="completedTodos" v-bind:todo="todos"/>
                <completed-tasks v-if="this.activeWin === 'completed'" v-bind:complete="completedTodos" v-bind:todo="todos"/>
            </div>
        </div>
    </div>
</template>

<script>
    import CurrentTasks from "@/components/CurrentTasks";
    import TaskAdder from "@/components/TaskAdder";
    import CompletedTasks from "@/components/CompletedTasks";
    export default {
        components: {CurrentTasks, TaskAdder, CompletedTasks},
        data() {
            return {
                todos: [],
                completedTodos: [],
                activeWin: 'current'
            }
        },
        mounted() {
            if (localStorage.toDoLists ) {
                const parse = JSON.parse(localStorage.toDoLists);

                parse.forEach((e) =>{
                    this.todos.push(e)
                })
            }
            if (localStorage.completedTodos) {
                const parse = JSON.parse(localStorage.completedTodos);

                parse.forEach((e) =>{
                    this.completedTodos.push(e)
                })
            }
        },
        methods: {
            switchViews(e, arg) {
                if (arg) {
                    this.activeWin = e
                    return;
                }
                this.activeWin = e.target.id
            }
        }

    }
</script>

<style>
body {
  background-color: black;
  color: white;
  font-family: 'Roboto', sans-serif;
  font-size: xx-large;
}


</style>
