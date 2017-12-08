<template lang='pug'>
.ni-notifications
  notification(v-for='notification in notifications'
    :key='notification.time'
    :data='notification'
    :theme='theme')
</template>

<script>
import Notification from './Notification'
export default {
  name: 'ni-notifications',
  components: {
    Notification
  },
  props: ['notifications', 'theme']
}
</script>

<style lang='stylus'>
.ni-notifications
  position: fixed
  top: 0
  right: 0
  z-index: 2000
  display: flex
  flex-flow: column nowrap
  align-items: stretch
  width: 100vw

.ni-notification
  margin-bottom: 0.5rem

.ni-notification:last-of-type
  margin-bottom: 0

@media screen and (min-width: 720px)
  .ni-notifications
    max-width: 30rem
</style>
