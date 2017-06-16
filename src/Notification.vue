<template>
  <div :class="cssClass" @click="deactivate" v-if="active">
    <header :style="headerStyle">
      <i :class="'icon fa fa-' + data.icon" v-if="data.icon"></i>
      <div class="title" v-if="data.title">{{ data.title }}</div>
      <menu>
        <div class="time" v-if="data.time">{{ fromNow }}</div>
        <i class="close fa fa-times-circle"></i>
      </menu>
    </header>
    <div class="body">{{ data.body }}</div>
  </div>
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
      if (this.data.type) value += `ni-notification-${this.data.type}`
      if (this.theme) value += `ni-notification-theme-${this.theme}`
      return value
    },
    headerStyle () {
      if (!this.data.type && this.color) {
        return {
          background: this.color
        }
      }
    }
  },
  data () {
    return {
      duration: 5000,
      active: true
    }
  },
  methods: {
    deactivate () {
      // console.log('destroying myself!')
      this.active = false
    },
    setDeactivation () { if (!this.data.layout || this.data.layout === 'banner') {
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
  props: ['data', 'color']
}
</script>

<style src="./Notification.css"></style>
