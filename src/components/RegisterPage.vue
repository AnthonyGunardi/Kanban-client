<template>
    <div class="mt-3">
        <h2>Register</h2>
        <form class="p-2" @submit.prevent="register">
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text">
                        <i class="material-icons">email</i>
                    </span>
                </div>
                <input type="email" class="form-control" placeholder="Email" v-model="email">
            </div>
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text">
                        <i class="material-icons">lock</i>
                    </span>
                </div>
                <input type="password" class="form-control" placeholder="Password" v-model="password">
            </div>
            <button type="submit" class="btn btn-primary">
                Register
            </button>
        </form>
        <p>Already have an account? <a href="" @click.prevent="toLogin">Just login.</a></p>
    </div>
</template>

<script>
export default {
    name: 'RegisterPage',
    data() {
        return {
            email: '',
            password: ''
        }
    },
    methods: {
        toLogin() {
            this.$emit("changeStatus","login")
            this.email = ''
            this.password = ''
        },
        register() {
            this.$emit("register", this.email, this.password)
            this.email = ''
            this.password = ''
        }
    }
}
</script>

<style>

</style>