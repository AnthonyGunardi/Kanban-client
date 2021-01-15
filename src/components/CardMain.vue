<template>
    <div>
        <div class="card-body text-left">
            <p>
                {{task.title}}
            </p>
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
        <div class="card-footer justify-content-beetwen bg-dark" v-if="task.UserEmail == loggedIn">
            <button class="btn btn-link bg-dark btn-sm" 
                @click.prevent="changeCategory(category.name, 'back', task.id)" 
                v-if="category.name != 'Backlog'"
            ><i class="material-icons">west</i></button>

            <button class="btn btn-link bg-dark btn-sm" 
            @click.prevent="changeIsEdit"
            ><i class="material-icons">edit</i></button>
            
            <button class="btn btn-link bg-dark btn-sm" 
                @click.prevent="destroy"
            ><i class="material-icons">delete</i></button>
            
            <button class="btn btn-link bg-dark btn-sm" 
                @click.prevent="changeCategory(category.name, 'next', task.id)"
                v-if="category.name != 'Completed'"
            ><i class="material-icons">east</i></button>
        </div>
    </div>
</template>

<script>
import moment from 'moment'

export default {
    name: 'CardMain',
    data () {
        return {
            loggedIn: localStorage.getItem('email')
        }
    },
    props: ['category', 'task'],
    methods: {
        destroy() {
            this.$emit("destroy", this.task.id)
        },
        changeIsEdit() {
            this.$emit("changeIsEdit", "yes")
        },
        changeCategory(base, change, id) {
            const arrCategory = ['Backlog', 'Todo', 'Doing', 'Completed']
            let index = arrCategory.findIndex(e => e == base)
            if (change == 'back') {
                index -= 1
            } else {
                index += 1
            }
            this.$emit("changeCategory", arrCategory[index], id)
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