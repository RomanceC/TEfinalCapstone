<template>
  <div class="testbox">
    <form class="form-signin" @submit.prevent="addToWorkout()">

      <template>
        <div>
          <v-container fluid grid-list-md>
            <v-layout row wrap>
              <v-flex class="ml-8" style="  max-width: 320px; 
  min-width: 320px;" xs12 sm6 md4>
                <v-card class="mx-auto my-12 pl-3">
                  <v-card-title style="font-weight: bold;">Update Workout</v-card-title>
                  <div class="alert" role="alert" v-if="createError === true">
                    Unable to edit exercise.
                  </div>
                  <div class="alert alert-success" role="alert" v-if="this.$route.query.exercise">
                    Exercise edited.
                  </div>
                  <div class="ml-9">
                    <select v-model="name" v-on:click="selectWorkout(name)" name="drop-down" placeholder="Exercise Type"
                      required>
                      <option value="" selected="selected">-- Workout Names --</option>
                      <option v-for="workout in $store.state.workouts" :key="workout.workoutId">
                        {{ workout.workoutName }}
                      </option>

                    </select>
                  </div>

                  <v-divider class="mx-4"></v-divider>
                  <template>
                    <v-container fluid class="my-4">
                      <v-layout justify-space-between>
                        <v-btn left color="success" fab medium dark v-on:click="addToWorkout()" class="ml-4">
                          <v-icon>mdi-check</v-icon>
                        </v-btn>
                        <v-btn right color="primary" fab medium dark v-on:click="hideForm()" class="mr-7">
                          <v-icon>mdi-cancel</v-icon>
                        </v-btn>
                      </v-layout>
                    </v-container>
                  </template>
                </v-card>
              </v-flex>
            </v-layout>
          </v-container>
        </div>
      </template>

    </form>
  </div>
</template>

<script>
import WorkoutService from '../services/WorkoutService';


export default {
  name: "add-workout",
  createError: false,
  data() {
    return {
      createError: false,

      name: {},

      exercise: {},

      selectedWorkoutId: 0,

    }
  },

  created() {
    this.exercise = this.$store.state.selectedExercise
  },


  methods: {
    hideForm() {
      this.$store.state.showAddWorkout = false;
    },

    addToWorkout() {
      let updatedWorkout = this.$store.state.selectedWorkout[0].exercises.unshift(this.exercise)
      this.$store.commit('UPDATE_WORKOUT', updatedWorkout)
      console.log(this.selectedWorkoutId[0].workoutId)
      let obj = {
        workoutId: this.selectedWorkoutId[0].workoutId,
        exerciseId: this.exercise.id
      }
      WorkoutService.sendExercises(obj.workoutId, obj).then((response) => {
        if (response.status == 200) {
          this.updateSuccess = true;
          this.hideForm();
        }
      })
    },

    selectWorkout(name) {

      let workoutToAppend = this.$store.state.workouts.filter((workout) => {
        return workout.workoutName === name
      });
      this.selectedWorkoutId = workoutToAppend;

      this.$store.commit('SELECT_WORKOUT', workoutToAppend)
    }
  }
};
</script>

<style scoped>
h1 {
  font-size: 32px;
  font-weight: 300;
  color: #4c4c4c;
  text-align: center;
  padding-top: 10px;
  margin-bottom: 10px;
  max-width: 100%;
}

.testbox {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin: 20px auto;
}

.alert {
  box-shadow: 1px 2px 5px black;
  text-align: center;
  font-size: 14px;
  font-weight: 600;
  background-color: rgb(240, 52, 52);
  color: #ebebeb;
  border-radius: 5px;
  animation: shake;
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  perspective: 1000px;
}

@keyframes shake {

  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}

hr {
  color: #f86c6c;
  opacity: 0.3;
}

form {
  margin: 0 30px;
  max-width: 100%;
}

input[type="text"] {
  width: 70%;
  height: 39px;
  border-radius: 0px 4px 4px 0px/5px 5px 4px 4px;
  background-color: #fff;
  box-shadow: 1px 2px 5px black;
  border: solid 1px #cbc9c9;
  margin-left: -5px;
  margin-top: 20px;
  padding-left: 10px;
  max-width: 100%;
}

#muscle-group {
  margin-top: 10px;
}

select {
  width: 90%;
  height: 39px;
  border-radius: 0px 4px 4px 0px/5px 5px 4px 4px;
  background-color: #fff;
  box-shadow: 1px 2px 5px black;
  border: solid 1px #cbc9c9;
  margin-left: -5px;
  margin-top: 20px;
  padding-left: 10px;
  max-width: 100%;
}


textarea {
  border-radius: 0px 4px 4px 0px/5px 5px 4px 4px;
  background-color: #fff;
  box-shadow: 1px 2px 5px black;
  border: solid 1px #cbc9c9;
  margin-top: 13px;
  margin-left: 10px;
  padding-left: 10px;
  max-height: 200px;
  max-width: 90%;
}

input {
  margin-bottom: 15px;
}

.send {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
  margin: 90px;
}

@media (max-width: 340px) {
  .testbox {
    max-width: 70vw;
  }

}
</style>