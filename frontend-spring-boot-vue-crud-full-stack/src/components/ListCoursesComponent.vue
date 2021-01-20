<template>
  <div class="container">
      <h3>All Courses</h3>
      <div v-if="message" class="alert alert-success">
          {{ message }}
      </div>
      <div class="container">
          <table class="table">
              <thead>
                  <tr>
                      <th>Id</th>
                      <th>Description</th>
                  </tr>
              </thead>
              <tbody>
                 <tr v-for="course in courses" :key="course.id">
                        <td>{{ course.id }}</td>
                        <td>{{ course.description }}</td>
                        <td>
                            <button class="btn btn-warning" @click="deleteCourseClicked(course.id)">
                                Delete
                            </button>
                        </td>
                        <td>
                            <button class="btn btn-sucess" @click="updateCourseClicked(course.id)">
                                Update
                            </button>
                        </td>
                  </tr>
              </tbody>
          </table>
      <div class="row">
        <button class="btn btn-success" @click="addCourseClicked()"> Add </button>
      </div>
  </div>
  </div>
</template>

<script>
import CourseDataService from '../service/CourseDataService';
export default {
    name: "CoursesList",
    data() {
        return {
            courses: [],
            message: null,
            INSTRUCTOR: "in28minutes"
        };
    },
    methods: {
        refreshCourses() {
            CourseDataService.retrieveAllCourses(this.INSTRUCTOR)
            .then(response => {
                this.courses = response.data;
            });
        },
        deleteCourseClicked(id) {
            CourseDataService.deleteCourse(this.INSTRUCTOR, id)
            .then(response => {
                this.message = `Delete of course ${id} Sucessful`;
                console.log(response.data)
                this.refreshCourses();
            })
            .catch(() => console.log("Can’t access  response. Blocked by browser?"))
        },
        updateCourseClicked(id) {
            this.$router.push(`/courses/${id}`)
        },
        addCourseClicked() {
            this.$router.push(`/courses/-1`);
        }
    },
    created() {
        this.refreshCourses();
    }
};
</script>

<style>

</style>