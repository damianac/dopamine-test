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
import axios from 'axios'

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
      apiResponse: [{ //input I got from mail
        id: 1321,
        type: 'text',
        title: 'Test notification',
        text: 'Test text notification',
        expires: 3600
      },{
        id: 4322,
        type: 'bonus',
        title: 'You win a bonus!',
        requirement: 'Deposit $50 to win',
        expires: 3600
      },{
        id: 5453,
        type: 'Promotion',
        image: 'https://www.freeiconspng.com/uploads/leistungen-promotion-icon-png-0.png',
        title: '%30 off on sports betting',
        link: 'https://www.google.com/',
      },{
        id: 5236,
        type: 'text',
        title: 'Test notification',
        text: 'Test text notification',
        expires: 5
      }],
    }
  },
  methods: {
    removeNotification: function(notification, afterMilliseconds = 0) {
      setTimeout(() => {
        this.notifications.map((item, index) => {
          if(item.id === notification.id) {
            this.notifications.splice(index, 1)
          }
        })
        if(notification.type !== "bonus")
          this.notificationCount--
      }, afterMilliseconds);
    },
    addNotification: function(notification) {
      this.notifications.push(notification)
      if(notification.type !== "bonus")
          this.notificationCount++
    },
    fetchNotifications: function(response) {
      this.notificationCount = 0;
      response.map((notification) => {
        let foundNotification = 0
        let notificationIndex = this.notifications.length - 1
        this.notifications.map((item, index) => {
          if(item.id === notification.id) {
            foundNotification = true
            notificationIndex = this.notiifications.length
          }
        })
        // checks if id already exists in notifications
        if(!foundNotification)
          this.addNotification(notification)
        else {
          // checks if there's a notification with the given id, but has some different values
          if(JSON.stringify(foundNotification) !== JSON.stringify(notification)) {
            this.notifications[notificationIndex] = this.notification
          }
        }
        if(typeof notification.expires !== 'undefined')
          this.removeNotification(notification, notification.expires)
      });

      // now we just need to check if there are some unnecessary notifications left
      let foundNotification;
      this.notifications.map((notification, index) => {
        foundNotification = false
        this.notifications.map((item) => {
          if(item.id === notification.id) {
            foundNotification = true
          }
        })
        if(!foundNotification)
          this.removeNotification(notification)
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

<style lang="scss">
body {
  .bv-example-row {
    div {
      &:first-child {
        cursor: pointer;
        display: block;
        background-color: blueviolet;
        text-align: center;
        color: white;
      }
      &.notification {
        margin-top: 10px;
        -webkit-box-shadow: 3px 3px 5px 0px #ccc;  /* Safari 3-4, iOS 4.0.2 - 4.2, Android 2.3+ */
        -moz-box-shadow:    3px 3px 5px 0px #ccc;  /* Firefox 3.5 - 3.6 */
        box-shadow:         3px 3px 5px 0px #ccc;  /* Opera 10.5, IE 9, Firefox 4+, Chrome 6+, iOS 5 */
      }
    }
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }
}
</style>