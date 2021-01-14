<template>
  
</template>

<script>
import axios from 'axios'
import Vue from 'vue'

export default {
    name: 'App',
    data() {
        return {
            pageName: 'landingpage',
            tasks: [],
            loginState: 'login'
        };
    },
    methods: {
<<<<<<< HEAD

=======
        changeStatus(input) {
            this.loginState = input
        },
        login(email, password) {
            axios({
                method: 'post',
                url: 'https://kanban-glasgow-server.herokuapp.com/login',
                data: {
                    email,
                    password
                }
            })
                .then(response => {
                    localStorage.setItem('access_token', response.data.access_token)
                    localStorage.setItem('email', response.data.email)
                    this.pageName = 'mainpage'
                    this.fetchTasks()
                })
                .catch(error => {
                    this.showAlert('error', 'Error', error.response.data.message)
                })
        },
        register(email, password) {
            axios({
                url:'https://kanban-glasgow-server.herokuapp.com/register',
                method:'POST',
                data: {
                    email,
                    password
                }
            })
                .then(response => {
                    this.showAlert('success', 'Success', `Success register ${response.data.email}`)
                    this.loginState = 'login'
                })
                .catch(error => {
                    let errors
                    if (Array.isArray(error.response.data)) {
                        errors = error.response.data.join(', ')
                    } else {
                        errors = error.response.data.errors
                    }
                    this.showAlert('error', 'Error', errors)
                })
        },
        logout(input) {
            this.pageName = input
            this.showAlert('success', 'Success', `Success Logout`)
        },
        fetchTasks() {
            axios({
                url:'https://kanban-glasgow-server.herokuapp.com/tasks',
                method:'GET',
                headers: {
                    access_token: localStorage.getItem('access_token')
                }
            })
                .then(response => {
                    this.tasks = response.data.tasks
                })
                .catch(error => {
                    this.showAlert('error', 'Error', error.response.data.message)
                })
        },
        destroy(id) {
            this.$swal({
                icon: 'warning',
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                reverseButtons: true,
                confirmButtonText: 'Yes, delete it!'
            })
                .then((result) => {
                    if (result.isConfirmed) {
                        return axios({
                            url:`https://kanban-glasgow-server.herokuapp.com/tasks/${id}`,
                            method: 'DELETE',
                            headers: {
                                access_token: localStorage.getItem('access_token')
                            }
                        })
                    }
                })
                .then(response => {
                    this.showAlert('success', 'Success', response.data.message)
                    this.fetchTasks()
                })
                .catch(error => {
                    this.showAlert('error', 'Error', error.response.data.message)
                })
        },
        update(id, title, category) {
            axios({
                url:`https://kanban-glasgow-server.herokuapp.com/tasks/${id}`,
                method: 'PUT',
                headers: {
                    access_token: localStorage.getItem('access_token')
                },
                data: {
                    title,
                    category
                }
            })
                .then(response => {
                    this.fetchTasks()
                })
                .catch(error => {
                    this.showAlert('error', 'Error', error.response.data.message)
                })
        },
        googleSignIn(googleUser) {
            const id_token = googleUser.getAuthResponse().id_token
            axios({
                url:`https://kanban-glasgow-server.herokuapp.com/googleLogin`,
                method:'POST',
                data: {
                    google_token: id_token
                }
            })
                .then(response => {
                    localStorage.setItem('access_token', response.data.access_token)
                    localStorage.setItem('email', response.data.email)
                    this.pageName = 'mainpage'
                    this.fetchTasks()
                })
                .catch(error => {
                    this.showAlert('error', 'Error', error.response.data.message)
                })
        },
        addTask(title, category) {
            axios({
                url:'https://kanban-glasgow-server.herokuapp.com/tasks',
                method:'POST',
                data: {
                    title,
                    category
                },
                headers: {
                    access_token: localStorage.getItem('access_token')
                }
            })
                .then(response => {
                    this.fetchTasks()
                })
                .catch(error => {
                    this.showAlert('error', 'Error', error.response.data.message)
                })
        },
        showAlert(icon, title, text) {
            // Use sweetalert2
            this.$swal({
                icon,
                title,
                text
            });
        },
        changeCategory(category, id) {
            axios({
                url:`https://kanban-glasgow-server.herokuapp.com/tasks/${id}`,
                method: 'PATCH',
                headers: {
                    access_token: localStorage.getItem('access_token')
                },
                data: {
                    category
                }
            })
                .then(response => {
                    this.fetchTasks()
                })
                .catch(error => {
                    this.showAlert('error', 'Error', error.response.data.message)
                })
        }
>>>>>>> 29f91e9... create template & methods in App.vue
    },
    components: {
      
    }
}
</script>

<style>

</style>