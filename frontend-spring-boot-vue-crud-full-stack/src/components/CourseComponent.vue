<template>
  <div>
      <h1>Course</h1>
        <div class="container">
            <form @submit="validateAndSubmit">
                <div v-if="errors.length">
                    <div class="alert alert-warning" :key="index" v-for="(error, index) in errors">{{ error }}</div>
                </div>
                <fieldset class="form-group">
                    <label>Id</label>
                    <input type="text" class="form-control" v-model="id" disabled>
                </fieldset>
                <fieldset class="form-group">
                    <label>Description</label>
                    <input type="text" class="form-control" v-model="description">
                </fieldset>
                <button class="btn btn-success">Save</button>
            </form>
        </div>
  </div>
</template>

<script>
import CourseDataService from '../service/CourseDataService'
export default {
    name: "courseDetails",
    data() {
        return {
            description: '',
            INSTRUCTOR: 'in28minutes',
            errors: []
        };
    },
    computed: {
        id() {
            return this.$route.params.id;
        }
    },
    methods: {
        refreshCourseDetails() {
            CourseDataService.retrieveCourse(this.INSTRUCTOR, this.id)
            .then(response => {
                this.description = response.data.description;
            });
        },
        validateAndSubmit(e) {
            e.preventDefault();
            this.errors = [];
            if(!this.description) {
                this.errors.push("Enter valid values");
            } else if(this.description.length < 5) {
                this.errors.push("Enter atleast 5 characters in Description");
            } 

            if(this.errors.length === 0) {
                if (this.id === -1) {
                    CourseDataService.createCourse(this.INSTRUCTOR, {
                        description: this.description
                    })
                    .then(() => {
                        this.$router.push('/courses');
                    });
                } else {
                    CourseDataService.updateCourse(this.INSTRUCTOR, this.id, {
                        id: this.id,
                        description: this.description
                    })
                    .then(() => {
                        this.$router.push('/courses')
                    })
                }
            }
        },
    },
    created() {
        this.refreshCourseDetails();
    }
}
</script>

<style>

</style>