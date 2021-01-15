<template>
    <!-- main tasks -->
    <div class="col-md-3">
            <div class="p-2 shadow rounded" :class="category.style">
                <nav class="navbar justify-content-between">
                    <h4>{{category.name}}</h4>
                    <div><a @click="showAddForm('true')"><i class="material-icons kanban-add">add_box</i></a></div>
                </nav>
                <CardAdd
                    v-if="showAdd == 'true'"
                    @showAddForm="showAddForm"
                    :category="category"
                    @addTask="addTask"
                ></CardAdd>
                <Card
                    v-for="task in tasksByCategory" :key="task.id"
                    :task="task"
                    :category="category"
                    @destroy="destroy"
                    @update="update"
                    @changeCategory="changeCategory"
                ></Card>
            </div>
    </div>    
    <!-- main tasks -->
</template>

<script>
import Card from './Card'
import CardAdd from './CardAdd'

export default {
    name: 'Category',
    props: ['tasks', 'category'],
    data () {
        return {
            showAdd:''
        }
    },
    methods: {
        destroy(id) {
            this.$emit("destroy", id)
        },
        update(id, title, category) {
            this.$emit("update", id, title, category)
        },
        showAddForm(value) {
            this.showAdd = value
        },
        addTask(title, category) {
            this.$emit("addTask", title, category)
        },
        changeCategory(category, id) {
            this.$emit("changeCategory", category, id)
        }
    },
    components: {
        Card,
        CardAdd
    },
    computed: {
        tasksByCategory () {
            return this.tasks.filter(task => task.category == this.category.name)
        }
    }
}
</script>

<style>

</style>