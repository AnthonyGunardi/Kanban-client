<template>
    <div>
        <LandingPage 
            v-if="pageName == 'landingpage'"
            @login="login"
            @register="register"
            @googleSignIn="googleSignIn"
            :loginState=loginState
            @changeStatus="changeStatus"
        ></LandingPage>
        <MainPage 
            v-if="pageName == 'mainpage'"
            @logout="logout"
            :tasks="tasks"
            @destroy="destroy"
            @update="update"
            @addTask="addTask"
            @changeCategory="changeCategory"
        ></MainPage>
        <Footer></Footer>
    </div>
</template>

<script>
import LandingPage from './components/LandingPage'
import MainPage from './components/MainPage'
import Footer from './components/Footer'
import axios from 'axios'
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'
import VueSweetalert2 from 'vue-sweetalert2';

// If you don't need the styles, do not connect
import 'sweetalert2/dist/sweetalert2.min.css';
// Install VueSweetalert2
Vue.use(VueSweetalert2);

// Install BootstrapVue
Vue.use(BootstrapVue)
// Optionally install the BootstrapVue icon components plugin
Vue.use(IconsPlugin)

import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

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
    },
    components: {
        LandingPage,
        MainPage,
        Footer
    },
    created () {
        if (localStorage.getItem('access_token')) {
            this.pageName = 'mainpage'
            this.fetchTasks()
        } else {
            this.pageName = 'landingpage'
        }
    }
}
</script>

<style>

</style>