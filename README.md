# vue-notifications

**VueNotifications** - agnostic non-blocking notifications library, that allow you to use notifications in declaration style.

----

##Instalation

via npm:

```shell
npm i vue-notifications --save
```

via bower:

```shell
bower i vue-notifications --save
```
or download [latest release][1]

include in project:

```JS
import VueNotifications from 'vue-notifications' 
```

```JS
Vue.use(VueNotifications, options)
```

##Setup and configuraton

**Attention:** By default VueNotification send all messages _to console_. To activate non-blocking notifiction you've got to use third-party library, like toasr. I suggest you to use [mini-toastr][2] (`npm i mini-toastr --save`)

```JS
//Include Plugin in project
import VueNotifications from 'vue-notifications'
//Include mini-toaster (or any other UI-notification library
import miniToastr from 'mini-toastr'

//Here we setup messages output to `mini-toastr`
function toast ({title, message, type, timeout, cb}) {
  return miniToastr[type](message, title, timeout, cb)
}

//Binding for methods .success(), .error() and etc. You can specify and map your own methods here
const options = {
  success: toast,
  error: toast,
  info: toast,
  warn: toast
}

//Activate plugin
Vue.use(VueNotifications, options)
```

```JS
//and if you would use miniToastr you have to init in in your App.vue
import miniToastr from 'mini-toastr'

//in 'ready section
//  ...
    ready () {
      miniToastr.init(miniToastrConfig)//'miniToastrConfig' is optionl here
    },
//  ...
```

If you want to setup VueNotification's global configuration, you can do it simple:

```JS
VueNotifications.config.timeout = 4000
Vue.use(VueNotifications, options)
```

##Usage

You've got to specify notification config:

```JS
export default {
    name: 'DemoView',
    data () {
      //...
    },
    computed: {
      //...
    },
    methods: {
      //...
    },
    notifications: {
      showLoinError: {
        title: 'Login Failed',
        message: 'Failed to authenticate',
        type: 'error' //Default: 'info', also you can use VueNotifications.type.error instead of 'error'
      }
    },
    vuex: {
      //...
    }
  }
```
Now you can call `this.showLoinError()` in any place of this page (i.e. in methods, or whatever).

You also can call `.success()`, `.error()` and other methods directly (for example in JavaScript files):

some.js:

```JS
  import VueNotifications from 'vue-notifications'
  VueNotifications.error({message: 'Some Error'})
```

##Options

**VueNotification** can work fine with any of your custom options, but by default it would be:

 - `title` - `String`, default `undefined`: Notification's title;
 - `message` - `String`, default `undefined`: Notification's body message. Normally should be set up;
 - `timeout` - `Number`, default `3000`: time before notifications gone;
 -  `cb` - `Function`, defuault: `undefined`: CallBack function;

####How to add custom field?

Simple: `this.showLoinError({consoleMessage: 'let it be in console'})`. You've passed a custom config here (`{consoleMessage: 'let it be in console'}`) that will be merged with config from `notifications.showLoinError` and with `global config` via `Object.assign` (beware of shallow copy).

As other option, you can specify as much custom fields as you want in `notifications` section:

```JS
      //...
    notifications: {
      showLoinError: {
        message: 'Failed to authenticate',
        type: 'error', //Also you can use VueNotifications.type.error instead of 'error'
        consoleMessage: 'let it be in console',
        consoleMessage2: 'let it be in console too',
        //etc
      }
    }
      //...
```

And do whatever you want after that:

```JS
function toast ({title, message, type, timeout, cb, consoleMessage}) {
  if (consoleMessage) console[type](consoleMessage) //Here we go!
  return miniToastr[type](message, title, timeout, cb)
}

const options = {
  success: toast,
  error: toast,
  info: toast,
  warn: toast
}

Vue.use(VueNotifications, options)
```

###ROADMAP:

1. Add native support for `computed` properties.

##Options


##License

MIT License

Copyright (c) 2016 Sergey Panfilov

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[1]: https://github.com/se-panfilov/vue-notifications/releases
[2]: https://github.com/se-panfilov/mini-toastr
