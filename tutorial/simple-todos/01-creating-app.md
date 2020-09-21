---
title: "1: Creating the app"
---

> This tutorial assumes basic knowledge of Vue. We recommend you to follow the Vue tutorial first and then follow up with this tutorial to learn the Meteor specifics for with Vue.

## 1.1: Install Meteor
First we need to install Meteor.

If you running on OSX or Linux run this command in your terminal:
```shell
curl https://install.meteor.com/ | sh
```

If you are on Windows:
First install [Chocolatey](https://chocolatey.org/install), then run this command using an Administrator command prompt:
```shell
choco install meteor
```

> You can check more details about Meteor installation [here](https://www.meteor.com/install)

## 1.2: Create Meteor Project

The easiest way to setup Meteor with Vue is by using the command `meteor create` with the option `--vue` and your project name:

```
meteor create --vue simple-todos-vue
```

Meteor will create all the necessary files for you. 

The files located in the `client` directory are setting up your client side (web), you can see for example `client/main.js` where the Vue App is initialized with a reference to the App component imported from `imports/ui/App.vue`

Also, check the `server` directory where Meteor is setting up the server side (Node.js), you can see the `server/main.js` is initializing your MongoDB database with some data. You don't need to install MongoDB as Meteor provides an embedded version of it ready for you to use.

You can now run your Meteor app using: 

```
meteor run
```

Don't worry, Meteor will keep your app in sync with all your changes from now on.

Your Vue code will be located inside the `imports/ui` directory, and `App.vue` file is the root component of your Vue To-do app.

Take a quick look in all the files created by Meteor, you don't need to understand them now but it's good to know where they are.

## 1.3: Create Task Component

You will make your first change now. Create a new file called `Task.vue` in your `ui` folder:

`imports/ui/Task.vue`
```js
<template>
  <li>{{ task }}</li>
</template>

<script>
export default {
  props: {
    task: String
  }
}
</script>
```

As this component will be inside a list, we just need to be returning the `li` element in our template section

## 1.4: Create Sample Tasks

As you are not connecting to your server and your database yet let's define some sample data in our App component and use it to render a list using the Task component for each item:

`imports/ui/App.vue`
```js
<template>
  <div>
    <h1>Welcome to Meteor!</h1>
 
    <ul>
      <task v-for="{id, text} in tasks" :key="id" :task="text" />
    </ul>
  </div>
</template>

<script>
import Task from './components/Task'
export default {
  components: {
    Task
  },
  data() {
    return [
      {_id: 1, text: 'First Task'},
      {_id: 2, text: 'Second Task'},
      {_id: 3, text: 'Third Task'},
    ]
  }
}
</script>
```

As specified on the `Task.vue` component, the `text` property needs to be a string, but you can change it to anything you want. Just be creative!

Remember to add the `key` property to your task. By default Vue uses an "in-place patch" to update items by index. If the order of the items would change, Vue might update the wrong item, because it uses the index to determine which items to update. By adding a unique key property, Vue knows which item to update. 

> You can read more about Vue and maintaining state [here](https://vuejs.org/v2/guide/list.html#Maintaining-State).

## 1.6 Mobile look

Let's see how your app is looking on Mobile. You can simulate a mobile environment `right clicking` your app in the browser (we are assuming you are using Google Chrome as it is the most popular browser today) and then `inspect`, this will open a new window inside your browser called `Dev Tools`. In the `Dev Tools` you have a small icon showing a Mobile device and a Tablet, see where this icon is:

<img width="500px" src="/simple-todos/assets/step01-dev-tools-mobile-toggle.png"/>

Click on it and then select the phone that you want to simulate and in the top bar.

> You can also check your app in your cellphone, you can connect to your App using your local IP in the navigation browser of your mobile browser.
>
> This command should print your local IP for you on Unix systems at least
`ifconfig | grep "inet " | grep -Fv 127.0.0.1 | awk '{print $2}'`

You are going to see something like this:

<img width="200px" src="/simple-todos/assets/step01-mobile-without-meta-tags.png"/>

As you can see everything is small, that is because we are not adjusting the view port for mobile devices, you can fix this and other similar issues by adding these lines to your `client/main.html` file, inside the `head` tag, after the `title`.

`client/main.html`
```html
  <meta charset="utf-8"/>
  <meta http-equiv="x-ua-compatible" content="ie=edge"/>
  <meta
      name="viewport"
      content="width=device-width, height=device-height, viewport-fit=cover, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no"
  />
  <meta name="mobile-web-app-capable" content="yes"/>
  <meta name="apple-mobile-web-app-capable" content="yes"/>
```

Now your app should look like this:

<img width="200px" src="/simple-todos/assets/step01-mobile-with-meta-tags.png"/>

> Review: you can check how your code should be in the end of this step [here](https://github.com/meteor/react-tutorial/tree/master/src/simple-todos/step01) 

In the next step we are going to work with MongoDB database to store our tasks.
