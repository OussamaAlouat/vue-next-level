<template>
    <div>
        <h1>Ooops !!</h1>
        <h3>The {{resource}} you're looking for is not here.</h3>
        <router-link :to="{name: 'event-list'}" >Back home page </router-link>
    </div>
</template>
<script>
export default {
  props: {
    resource: {
      type: String,
      required: true
    }
  }
}
</script>
