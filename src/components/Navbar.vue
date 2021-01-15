<template>
    <!-- navbar -->
        <nav class="navbar shadow-lg navbar-light kanban-navbar justify-content-between sticky-top">
            <h3 class="text-white">Simple Kanban Board <span class="nav-small">( {{currentLoggedIn}} )</span></h3>
            <a @click.prevent="logout">
                <GoogleLogin :params="params" :logoutButton=true class="btn btn-danger">Logout</GoogleLogin>
            </a>
        </nav>
    <!-- navbar -->
</template>

<script>
import GoogleLogin from 'vue-google-login';

export default {
    name: 'Navbar',
    data () {
        return {
            currentLoggedIn: localStorage.getItem('email'),
            params: {
                    client_id: "1055447405319-jc39f3pe7elh6r6cttum8r1c8t69ar5u.apps.googleusercontent.com"
            }
        }
    },
    methods: {
        logout() {
            localStorage.removeItem('access_token')
            localStorage.removeItem('email')
            this.$emit("logout", "landingpage")
        }
    },
    props: ['logoutButton'],
    components: {
        GoogleLogin
    }
}
</script>

<style>

</style>