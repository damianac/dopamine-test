# dopamine-test
A test project for Dopamine

# Tools used

I've used Vue (with vue-cli), SCSS and Bootstrap

# Guideline

Task's code is located in src/components/Notification.vue
At the moment, notifications are fetched only once you click on notification grid.

There was a synax error with the javascript object I've got from mail. I assumed that it was a mistake on your end and fixed it myself.

Since there was no api endpoint provided, I've just hard coded the object you provided. Anyways, API doesn't really seem the best method for notifications, websockets would be perfect for this.

As for the task I've got in mail:

```
- When a notification expires it should be removed from the notifications list. Some notifications do not have an expiration.
```
It wasn't mentioned in the mail how to interpret the time provided for expiration. I just assumed it was meant milliseconds. 



## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```


