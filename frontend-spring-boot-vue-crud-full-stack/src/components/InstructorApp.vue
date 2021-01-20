<template>
    <div>
        <h1>This is instructors test!</h1>
        <list-courses-component />
    </div>
</template>

<script>
import ListCoursesComponent from './ListCoursesComponent.vue'
export default {
    name: "InstructorApp",
    components: { ListCoursesComponent },
}
</script>

<style scoped>

</style>