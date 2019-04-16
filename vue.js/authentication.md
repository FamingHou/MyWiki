## Install vue-cookie

```console
npm install vue-cookie@1.1.4 --save
```
## load it in main.js

src/main.js
```javascript
// Require dependencies
var Vue = require('vue');
var VueCookie = require('vue-cookie');
// Tell Vue to use the plugin
Vue.use(VueCookie);
```

## Config Route Meta Field ```requiresAuth```

src/router/paths.js
```javascript
{
  path: '/',
  meta: {
    requiresAuth: true,
  },
  name: 'Root',
  redirect: {
    name: 'Dashboard'
  }
}
```  

## Set cookie

src/views/Login.vue
```javascript
methods: {
  login () {
    ...
    this.$cookie.set('token', 'the_value_of_token', { expires: '1M' }); // expires one month later
    ...
  }
}
```

## Check ```requiresAuth``` in the global navigation guard

src/router/index.js
```javascript
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!this.$cookie.get('token')) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // make sure to always call next()!
  }
})
```

## References

[Route Meta Fields](https://router.vuejs.org/guide/advanced/meta.html)
[vue-cookie](https://www.npmjs.com/package/vue-cookie)
