<template>
  <div>
    <h1>Create an Event</h1>
    <form @submit.prevent="createEvent">
      <base-select
        v-model="event.category"
        :options="categories"
        label="Select a category"
        :class="{ error: $v.event.category.$error }"
         @blur="$v.event.category.$touch()"
      />
      <template v-if="$v.event.category.$error">
        <p v-if="!$v.event.category.required"
          class="errorMessage"
        >
          Category is required
        </p>
      </template>
      <h3>Name & describe your event</h3>

      <base-input
        type="text"
        placeholder="Add a name"
        label="Title"
        v-model="event.title"
        class="field"
        :class="{ error: $v.event.title.$error }"
        @blur="$v.event.title.$touch()"
      />

      <template v-if="$v.event.title.$error">
        <p v-if="!$v.event.title.required"
          class="errorMessage"
        >
          Title is required
        </p>
      </template>

      <base-input
        type="text"
        placeholder="Add a description"
        label="Description"
        v-model="event.description"
        class="field"
        :class="{ error: $v.event.description.$error }"
        @blur="$v.event.description.$touch()"
      />

      <template v-if="$v.event.description.$error">
        <p v-if="!$v.event.description.required"
          class="errorMessage"
        >
          Description is required
        </p>
      </template>

      <h3>Where is your event?</h3>

      <base-input
        type="text"
        placeholder="Add a location"
        label="Location"
        v-model="event.location"
        class="field"
        :class="{ error: $v.event.location.$error }"
        @blur="$v.event.location.$touch()"
      />

      <template v-if="$v.event.location.$error">
        <p v-if="!$v.event.location.required"
          class="errorMessage"
        >
          Location is required
        </p>
      </template>

      <h3>When is your event?</h3>

      <div class="field">
        <label>Date</label>
        <datepicker 
          v-model="event.date"
          placeholder="Select a date"
          :class="{ error: $v.event.location.$error }"
          @opened="$v.event.location.$touch()"
        />
      </div>
      <template v-if="$v.event.date.$error">
        <p v-if="!$v.event.date.required"
          class="errorMessage"
        >
          Date is required
        </p>
      </template>

      <base-select
        v-model="event.time"
        :options="times"
        label="Select a time"
        class="field"
        :class="{ error: $v.event.time.$error }"
        @blur="$v.event.time.$touch()"
      />

      <template v-if="$v.event.time.$error">
        <p v-if="!$v.event.time.required"
          class="errorMessage"
        >
          Time is required
        </p>
      </template>

      <base-button
        buttonClass="-fill-gradient"
        type="submit"
        :disabled="$v.$anyError"
      >
        Submit
      </base-button>

      <p v-if="$v.$anyError" class="errorMessage">
        Please fill out the required field(s)
      </p>
    </form>
  </div>
</template>


<script>
import Datepicker from 'vuejs-datepicker'
import NProgress from 'nprogress'
import { required } from 'vuelidate/lib/validators'

export default {
  components: {
    Datepicker
  },
  data() {
    const times = []
    for (let i = 1; i <= 24; i++) {
      times.push(i + ':00')
    }
    return {
      times,
      categories: this.$store.state.categories,
      event: this.createFreshEventObject()
    }
  },
  validations: {
    event: {
      category: { required },
      title: { required },
      description: { required },
      location: { required },
      date: { required },
      time: { required }
    }
  },
  methods: {
    createEvent() {
      NProgress.start()
      this.$v.touch()
      if (!this.$v.$invalid) {
        this.$store
          .dispatch('event/createEvent', this.event)
          .then(() => {
            this.$router.push({
              name: 'event-show',
              params: { id: this.event.id }
            })
            this.event = this.createFreshEventObject()
          })
          .catch(() => {
            NProgress.done()
          })
      }
    },
    createFreshEventObject() {
      const user = this.$store.state.user.user
      const id = Math.floor(Math.random() * 10000000)

      return {
        id: id,
        user: user,
        category: '',
        organizer: user,
        title: '',
        description: '',
        location: '',
        date: '',
        time: '',
        attendees: []
      }
    }
  }
}
</script>

<style>
.field {
  margin-bottom: 24px;
}
</style>
