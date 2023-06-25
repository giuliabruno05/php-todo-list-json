<script >
import axios from 'axios';
export default {
    data() {
        return {
            todos: [],
            newtodos: {
                "todo": "",
                "status": false
            }
        }
    },
    methods: {
        onSubmit() {
            const url = "http://localhost/php-todo-list-json/todo-list-backend/newtodos.php";
            const data = this.newtodos;
            const headers = {
                headers: { 'Content-Type': 'multipart/form-data' }
            };
            if (this.newtodos.todo !== '') {
                axios.post(url, data, headers)
                    .then(res => {
                        const data = res.data;
                        this.todos = data;

                        this.newtodos.todo = "";
                    })
            }
        },
        deleteTodo(i) {
            const url = "http://localhost/php-todo-list-json/todo-list-backend/deleteTodo.php";
            const data = { "index": i };
            const headers = {
                headers: { 'Content-Type': 'multipart/form-data' }
            };
            axios.post(url, data, headers)
                .then(res => {
                    const data = res.data;
                    this.todos = data;
                });
        }
    },
    mounted() {

        axios.get("http://localhost/php-todo-list-json/todo-list-backend/todos.php")
            .then(res => {
                const data = res.data;
                this.todos = data;
                console.log(data);
            });
    }
}

</script>

<template>
    <h1>Todo List</h1>
    <div class="container">
        <ul>
            <li v-for="(todo, i) in todos " :key="i">
                {{ todo.todo }}
                <button @click="deleteTodo(i)"><i class="fa-regular fa-trash-can"></i></button>
            </li>
        </ul>
        <form @submit.prevent="onSubmit">
            <input type="text" name="todo" v-model="newtodos.todo">
            <input type="submit" value="inserisci">
        </form>
    </div>
</template>

<style lang="scss">
body {
    background-color: #001632;
    text-align: center;

    h1 {
        color: grey;
    }

    .container {
        background-color: white;
        width: 30%;
        margin: 0 auto;
        padding: 20px;

        ul {
            list-style: none;

            li {
                display: flex;
                justify-content: space-between
            }
        }
    }


}
</style>
