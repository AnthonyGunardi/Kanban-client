<template>
    <form @submit.prevent="update">
        <div class="card-body text-left">
            <div class="md-form mb-3">
                <input type="text" class="form-control" placeholder="Input Title" v-model="title">
            </div>

            <div class="md-form mb-3">
                <select class="custom-select form-control" v-model="selected">
                    <option value="Backlog">Backlog</option>
                    <option value="Todo">Todo</option>
                    <option value="Doing">Doing</option>
                    <option value="Completed">Completed</option>
                </select>
            </div>
            <hr>
            <footer class="blockquote-footer">
                by:
                <p>{{task.UserEmail}}</p>
            </footer>
            <footer class="blockquote-footer">created at: 
                <p>{{createdAtVisual}}</p>
            </footer>
            <footer class="blockquote-footer">
                last update:
                <p>{{updatedAtVisual}}</p>
            </footer>
        </div>
        <div class="card-footer text-right bg-dark">
            <button type="submit" class="btn btn-link bg-dark btn-sm">
                <i class="material-icons">done</i>
            </button>
            <button class="btn btn-link bg-dark btn-sm" @click.prevent="changeIsEdit">
                <i class="material-icons">close</i>
            </button>
        </div>
    </form>
</template>

<script>
import moment from 'moment'

export default {
    name: 'CardEdit',
    data () {
        return {
            title: this.task.title,
            selected: this.task.category
        }
    },
    props: ['task'],
    methods: {
        changeIsEdit() {
            this.$emit("changeIsEdit", "no")
        },
        update() {
            this.$emit("update", this.task.id, this.title, this.selected)
            this.$emit("changeIsEdit", "no")
        }
    },
    computed: {
        updatedAtVisual() {
            return moment(this.task.updatedAt).format('h:mm:ss a, Do MMMM YYYY')
        },
        createdAtVisual() {
            return moment(this.task.createdAt).format('h:mm:ss a, Do MMMM YYYY')
        }
        
    }
}
</script>

<style>

</style>