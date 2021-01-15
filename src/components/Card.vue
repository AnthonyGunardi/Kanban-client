<template>
    <div class="card shadow m-2 mb-3">
        <CardMain
        v-if="isEdit == 'no'"
        :category="category"
        :task="task"
        @destroy="destroy"
        @changeIsEdit="changeIsEdit"
        @changeCategory="changeCategory"
        ></CardMain>

        <CardEdit
        v-else
        :task="task"
        @changeIsEdit="changeIsEdit"
        @update="update"
        ></CardEdit>
    </div>
</template>

<script>
import CardMain from './CardMain'
import CardEdit from './CardEdit'

export default {
    name: 'Cards',
    data () {
        return {
            isEdit:'no'
        }
    },
    props: ['category', 'task'],
    methods: {
        destroy(id) {
            this.$emit("destroy", id)
        },
        changeIsEdit(input) {
            this.isEdit = input
        },
        update(id, title, category) {
            this.$emit("update", id, title, category)
        },
        changeCategory(category, id) {
            this.$emit("changeCategory", category, id)
        }
    },
    components: {
        CardMain,
        CardEdit
    }
}
</script>

<style>

</style>