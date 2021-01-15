<template>
    <!-- landing page -->
    <div class="container-fluid landing-page">
        <div class="row container-header">
            <div class="col-md-7 container-title text-center">
                <h1 class="title">Simple Kanban Board</h1>
            </div>
            <div class="col-md-5 container-login text-center">
                <LoginPage 
                    v-if="loginState == 'login'"
                    @changeStatus="changeStatus"
                    @login="login"
                ></LoginPage>
                <RegisterPage 
                    v-if="loginState == 'register'"
                    @changeStatus="changeStatus"
                    @register="register"
                ></RegisterPage>
                <p>Or Login With Google</p>
                <GoogleLogin :params="params" :renderParams="renderParams" :onSuccess="onSuccess" :onFailure="onFailure"></GoogleLogin>
            </div>
        </div>
    </div>
    <!-- landing page -->
</template>

<script>
import LoginPage from './LoginPage'
import RegisterPage from './RegisterPage'
import { GoogleLogin } from 'vue-google-login'

export default {
    name: 'LandingPage',
    data () {
        return {
            params: {
                    client_id: "1055447405319-jc39f3pe7elh6r6cttum8r1c8t69ar5u.apps.googleusercontent.com"
                },
            // only needed if you want to render the button with the google ui
            renderParams: {
                width: 250,
                height: 50,
                longtitle: true
            }
        }
    },
    methods: {
        changeStatus(param) {
            this.$emit("changeStatus", param)
        },
        login(email, password) {
            this.$emit("login", email, password)
        },
        register(email, password) {
            this.$emit("register", email, password)
        },
        onSuccess(googleUser) { 
            // This only gets the user information: id, name, imageUrl and email
            this.$emit("googleSignIn", googleUser)
        }
    },
    components: {
        LoginPage,
        RegisterPage,
        GoogleLogin
    },
    props: ['loginState','onFailure', 'logoutButton', 'onCurrentUser']
}
</script>

<style>

</style>