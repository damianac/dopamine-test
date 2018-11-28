<template>
  <div class="notification">
    <b-container class="bv-example-row">
      <b-row @click="isHidden = !isHidden" @click.once="fetchNotifications(apiResponse)">
          Notifications ({{notificationCount}})
      </b-row>
      <transition name="fade" v-for="notification in notifications" :key="notification.id">
        <b-row  v-show="!isHidden" class="notification">
            <h5>{{notification.title}}</h5>
        </b-row>
      </transition>
    </b-container>
  </div>
</template>
<script>
import {API_PROVIDED} from "./Notifications.js"
export default {
  name: "Notifications",
  props: {
  },
  data: function() {
    return {
      isHidden: true,
      notifications: [],
      notificationCount: 0,
      api: '/', // hardcoded, since we don't have API,
      apiResponse: API_PROVIDED,
    }
  },
  methods: {
    removeNotification: function(notification, afterMilliseconds = 0) {
      setTimeout(() => {
        this.notifications = this.notifications.filter((item) => item.id !== notification.id)
        if(notification.type !== "bonus")
          this.notificationCount--
      }, afterMilliseconds)
    },
    addNotification: function(notification) {
      this.notifications.push(notification)
      if(notification.type !== "bonus")
        this.notificationCount++
    },
    fetchNotifications: function(response) {
      this.notificationCount = 0
      let foundNotification = false;
      response.map((notification) => {
        foundNotification = 0
        foundNotification = this.notifications.some(item => item.id === notification.id)
        // checks if id already exists in notifications
        if(!foundNotification)
          this.addNotification(notification)
        else {
          // checks if there's a notification with the given id, but has some different values
          if(JSON.stringify(foundNotification) !== JSON.stringify(notification)) {
            this.notifications[this.notifications.indexOf(notification)] = this.notification
          }
        }
        if(typeof notification.expires !== 'undefined')
          this.removeNotification(notification, notification.expires)
      });

      // now we just need to check if there are some unnecessary notifications left
      this.notifications = this.notifications.filter(notification => {
        return this.notifications.some(item => item.id === notification.id)
      })
    }
  },
  mounted: function() {
    this.apiResponse.map(notification => {
      if(notification.type !== "bonus")
        this.notificationCount++;
    })
  }
}
</script>
<style src="./Notifications.scss" lang="scss"></style>