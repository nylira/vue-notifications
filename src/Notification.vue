<template lang='pug'>
div(:class='cssClass' @click='deactivate' v-if='active')
  header
    .icon(v-if='data.icon'): i.material-icons {{ data.icon }}
    .title(v-if='data.title') {{ data.title }}
    menu
      .time(v-if='data.time') {{ fromNow }}
      i.close.material-icons close
  .body {{ data.body }}
</template>

<script>
import moment from 'moment'
export default {
  name: 'ni-notification',
  computed: {
    fromNow () {
      return moment(this.data.time).fromNow()
    },
    cssClass () {
      let value = 'ni-notification'
      if (this.data.type) value += ` ni-notification-${this.data.type}`
      if (this.theme) value += ` ni-notification-theme-${this.theme}`
      return value
    }
  },
  data: () => ({
    duration: 5000,
    active: true
  }),
  methods: {
    deactivate () {
      // console.log('destroying myself!')
      this.active = false
    },
    setDeactivation () {
      if (!this.data.layout || this.data.layout === 'banner') {
        // notification active duration is 5 seconds - (time since creation)
        let activeDuration = this.duration - (Date.now() - this.data.time)

        // disable visibility if it's an old notification
        if (activeDuration < 0) {
          this.active = false
          return
        }

        // otherwise self destruct after duration
        setTimeout(this.deactivate, activeDuration)
      }
    }
  },
  mounted () {
    this.setDeactivation()
  },
  props: ['data', 'theme']
}
</script>

<style lang='stylus'>
@require '~variables'

.ni-notification
  background app-bg

  font-size 0.75rem
  cursor pointer
  user-select none
  margin 0.5rem 0.5rem 0

  border 2px solid bc
  border-radius 0.25rem
  shadow()

.ni-notification header
  display flex
  align-items center
  padding 0 0.375rem
  height 2em

  background app-fg

  .icon
    color bright
  .title
    color bright

.ni-notification.ni-notification-warning header .icon
  color warning

.ni-notification.ni-notification-error header .icon
  color danger

.ni-notification header .icon
  width 1rem
  display flex
  align-items center
  margin-right 0.5rem

.ni-notification header .title
  flex 1
  font-weight 500
  text-overflow ellipsis
  overflow hidden
  white-space nowrap
  padding-right 0.375rem

.ni-notification header menu
  color dim
  font-size 0.75rem

.ni-notification header menu .close
  display none

.ni-notification .body
  padding 0.375rem
  color txt

.ni-notification:hover menu .time
  display none

.ni-notification:hover menu .close
  display block

@media screen and (min-width 360px)
  .ni-notification
    font-size 0.875rem
    margin 0.625rem 0.625rem 0

  .ni-notification header
    padding 0 0.5rem

  .ni-notification .body
    padding 0 0.5rem

@media screen and (min-width 400px)
  .ni-notification
    margin 0.75rem 0.75rem 0

  .ni-notification header
    padding 0 0.75rem

  .ni-notification .body
    padding 0.75rem

@media screen and (min-width 720px)
  .ni-notification
    margin 1rem 1rem 0
</style>
