<template>
  <div class="ni-notifications">
    <notification
      v-for="notification in notifications"
      :data="notification"
      :color="color"
    >
    </notification>
  </div>
</template>

<script>
import Notification from './Notification'
export default {
  name: 'ni-notifications',
  components: {
    Notification
  },
  props: ['notifications', 'color']
}
</script>

<style src="./Notifications.css"></style>
