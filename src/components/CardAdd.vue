<template>
    <div class="card shadow m-2 mb-3">
        <form @submit.prevent="addTask">
            <div class="card-body text-left">
                <div class="md-form">
                    <input type="text" class="form-control" placeholder="Add New Title" v-model="title" autofocus>
                </div>
            </div>
            <div class="card-footer text-right bg-dark">
                <button type="submit" class="btn btn-link bg-dark btn-sm">
                    <i class="material-icons">done</i>
                </button>
                <button class="btn btn-link bg-dark btn-sm" @click.prevent="showAddForm">
                    <i class="material-icons">close</i>
                </button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    name: 'CardAdd',
    data () {
        return {
            title: ''
        }
    },
    props: ['category'],
    methods: {
        showAddForm() {
            this.$emit('showAddForm', '')
            this.title = ''
        },
        addTask() {
            this.$emit('addTask', this.title, this.category.name)
            this.title = ''
            this.$emit('showAddForm', '')
        }
    }
}
</script>

<style>

</style>