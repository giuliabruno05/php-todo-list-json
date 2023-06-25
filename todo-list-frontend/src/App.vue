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
        },
        toggleLine(indice) {
            // this.todos[indice].status = !this.todos[indice].status
            if (this.todos[indice].status === 'true') {
                this.todos[indice].status = 'false'
            } else {
                this.todos[indice].status = 'true'
            }
        },
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
                <span :class="todo.status === 'true' ? 'line-through' : ''" @click="toggleLine(i)">{{ todo.todo }}</span>
                <button @click="deleteTodo(i)"><i class="fa-solid fa-trash"></i></button>
            </li>
        </ul>
    </div>
    <form @submit.prevent="onSubmit">
        <input type="text" name="todo" v-model="newtodos.todo" placeholder="Inserisci un elemento...">
        <input class="submit" type="submit" value="Inserisci">
    </form>
</template>

<style lang="scss">
* {
    padding: 0;
    box-sizing: border-box;
    font-family: 'Inter', sans-serif;
}

body {
    background-color: #001632;
    text-align: center;

    h1 {
        color: grey;
    }

    .container {
        background-color: white;
        width: 35%;
        margin: 0 auto;
        border-radius: 8px;

        ul {
            list-style: none;


            li {
                display: flex;
                justify-content: space-between;
                padding: 10px 0;
                border-bottom: 1px solid lightgray;
                padding: 10px 20px;
                align-items: center;

                &:last-child {
                    border: hidden;
                }

            }

            .line-through {
                text-decoration: line-through;
                cursor: pointer;
            }

            button {
                background-color: #db3243;
                padding: 10px;
                color: white;
                border: hidden;
                border-radius: 5px;
            }
        }
    }

    input {
        border-radius: 3px 0 0 3px;
        padding: 8px;
        margin: 20px 0;
        border: 0.5px solid lightgray;
        width: calc(35vw - 65px);
    }

    .submit {
        color: #d19a15;
        background-color: #001632;
        border: 0.5px solid transparent;
        border-color: #d19a15;
        border-radius: 0 3px 3px 0;
        width: 65px;

    }

}
</style>
