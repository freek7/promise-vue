# promise-vue
Ultra simple component to work with Promise. 

### Installation
It may be installed  as [npm package](https://www.npmjs.com/package/promise-vue "npm")

    npm i promise-vue
	import VuePromiseComponent from 'promise-vue'
	
then register as component


    components: {
        "promise-vue": VuePromiseComponent,
      }
	  
### How to use
In promise You have 3 state
1. **pendding**
2. **resolved** 
3. **error**

So all you need it pass promise to `promise-vue` component
it looks like : 
```html
<promise-vue :delay="delay" :promise="examplePromise" tag="div">
	<template v-slot:pendding>
		<div>Loadding...</div>
	</template>

	<template v-slot:resolved="data">
		<pre>{{ data }}</pre>
	</template>

	<template v-slot:error="error">
		<pre>Opss... {{ error }}</pre>
	</template>
</promise-vue>
```

### Slots

- `v-slot:pendding` : shows when promise pending/loading
- `v-slot:resolved="data"` : scoped slot with received data
- `v-slot:error="error"` : scoped slot with error data

### Props
- `promise` : just promise
- `tag` : tag to wrap by default - div
- `delay` : delay  of minimum time to show loading slot by default - 0


------------

P.S. real example in **src\Example.vue**