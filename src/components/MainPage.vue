<template>
<!-- main page -->
    <div id="main-page" class="main-page">
        <Navbar
        @logout="logout"
        ></Navbar>
        <div class="container-fluid text-center mt-3 pb-3">
            <div class="row">
                <Category
                    v-for="category in categories" :key="category.id"
                    :category="category"
                    :tasks=tasks
                    @destroy="destroy"
                    @update="update"
                    @addTask="addTask"
                    @changeCategory="changeCategory"
                ></Category>
            </div>
        </div>
    </div>
</template>

<script>
import Navbar from './Navbar'
import Category from './Category'

export default {
    name: 'MainPage',
    data () {
        return {
            categories: [
                {
                    id: 1,
                    name: 'Backlog',
                    style: 'kanban-backlog' 
                },
                {
                    id: 2,
                    name: 'Todo',
                    style: 'kanban-todo' 
                },
                {
                    id: 3,
                    name: 'Doing',
                    style: 'kanban-done' 
                },
                {
                    id: 4,
                    name: 'Completed',
                    style: 'kanban-completed' 
                },
            ]
        }
    },
    props: ['tasks'],
    components: {
        Navbar,
        Category
    },
    methods: {
        logout(input) {
            this.$emit("logout", input)
            const auth2 = gapi.auth2.getAuthInstance();
                auth2.signOut().then(function () {
            });
        },
        destroy(id) {
            this.$emit("destroy", id)
        },
        update(id, title, category) {
            this.$emit("update", id, title, category)
        },
        addTask(title, category) {
            this.$emit("addTask", title, category)
        },
        changeCategory(category, id) {
            this.$emit("changeCategory", category, id)
        }
    }
}
</script>

<style>

</style>