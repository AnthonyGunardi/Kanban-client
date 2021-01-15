<template>
    <div class="mt-3">
        <h2>Login</h2>
        <form class="p-2" @submit.prevent="login">
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text">
                        <i class="material-icons">email</i>
                    </span>
                </div>
                <input type="email" class="form-control" placeholder="Email" autofocus v-model="user.email">
            </div>
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text">
                        <i class="material-icons">lock</i>
                    </span>
                </div>
                <input type="password" class="form-control" placeholder="Password" v-model="user.password">
            </div>
            <button type="submit" class="btn btn-primary">
                Login
            </button>
        </form>
        <p>Don't have an account? <a href="" @click.prevent="toRegister">Register first.</a></p>
    </div>
</template>

<script>
export default {
    name: 'LoginPage',
    data () {
        return {
            user: {
                email: '',
                password: ''
            }
        }
    },
    methods: {
        toRegister() {
            this.$emit("changeStatus","register")
            this.email = ''
            this.password = ''
        },
        login() {
            this.$emit("login", this.user.email, this.user.password)
            this.email = ''
            this.password = ''
        }
    }
}
</script>

<style>

</style>