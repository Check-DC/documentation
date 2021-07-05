
# Nuxt(Vue) Best Practices
The Vue lifecycle comes in handy and increases frontend development ETA, Nuxt comes along with special features as an addition to this lifecycle, these special features are powered by the `nuxt.config.js` and this can immediately get opinionated and complex to scale.

## Installation 
1. `yarn` is the package manager of choice.
2. Every project **must** start with the create nuxt app command:  
```
yarn create nuxt-app {project-name or (./)[i.e. current folder]}
```
3. After **2. above**, here are the answers to the interactive in compliance to our frontend projects:  
```
	Project name:  {name of folder by default} This should always have the format {projectname-customer-facing}

	Programming language: {javascript} This is the preferred option.

	Package manager: {yarn} This is the preferred option.

	UI framework: {Tailwind CSS} Utility first and it's none un-intrusive to the webflow exports and other primary css framework used. Helps with quick space adjustments and etc.

	Nuxt.js modules: {Axios and PWA} These two are the select choices.

	Linting tools: {Eslint} This is the only select choice. Prettier should never be used.

	Testing framework: {optional but if required: Jest} This is preferred choice.

	Rendering mode: {Single Page App} SSR is cool but extends development time due to extra lines of checks for client & server side also it is expressly not required if proper care is taken while writing a SEO friendly SPAs

	Deployment target: {Static (Static/Jamstack hosting)} This works well with Netlify.com our preferred CI/CD platform.

	Development tools: {jsconfig.json} This is need for the editor. To void cases where advance directories (path) are not easily read especially for javascript.

	Continuous integration: {none} No CI should be selected as Netlify will take care of all that.

	Version control system: {Git}
``` 

## Setup
1. The `.env` file should be the only file that has the environment variables.  
2. All functions responsible for HTTP request call should carry the same name as the action function in the `store/index.js`  
3.  The `store/index.js` should be setup as seen below:
```
	export const state = () => ({
		// key: value (optionally from local storage)
	})
	
	export  const  getters  =  {
		// get{StateKeyName}(state){}
	})
	
	export  const  mutations  =  {
		// functionName(state, payload{optional}{}
	})
	
	export  const  actions  =  {
		// functionName(context, payload{optional}){}
	})
	
```
4. The plugin folder should contain only the following files where applicable:  
```
- prototypes.js  
- filters.js  
- components.js  
- directives.js  
- axios.js  
- google-analytics.js  
```
5.  The `.eslintrc.js`  should be setup with the preset rules as seen below:
```
module.exports = {
	root: true,
	env: {
		browser: true,
		node: true
	},
	parserOptions: {
		parser: 'babel-eslint'
	},
	extends: ['@nuxtjs', 'plugin:nuxt/recommended'],
	plugins: [],
	// add your custom rules here
	rules: {
		indent: 0,
		'vue/no-v-html': 0,
		'vue/html-self-closing': 0,
		'space-before-function-paren': 0,
		'no-console': 0,
		'operator-linebreak': 0,
		'sort-keys': [0, 'asc', { caseSensitive: true, natural: true, minKeys: 2 }],
		'vue/attributes-order': [
			2,
			{
				alphabetical: true
			}
		],
		'vue/no-vue-html': 0,
		'vue/html-closing-bracket-newline': [
			'error',
			{
				singleline: 'never',
				multiline: 'never'
			}
		]
	},
	overrides: [
		{
			files: [''], // *filename
			rules: {
				'eslint-disable-file': 'off'
			}
		}
	]
}
```

## Vue File
1. All functions in the methods block must be in **camelCase**.  
2. All functions must be prefixed with an action word e.g **fetchProducts**, **storeProduct**  
3. All logical functions should come first in the methods 
4. All post/patch/put request functions should come just after **3. above**
5. All get request functions should come just after **4. above**
6. All function that dispatches a **vuex** action should carry the exact same name of the action it dispatches.
```
storeProduct() {
	this.$store.dispatch('storeProduct', this.product)
}
```
7. Child components name should always be of two words no more no less:  
	This -> `PrimaryButton`  
	not this -> `PrimaryButtonRed`  
	nor this-> `Primary`
	
8. Child components should be consumed with just the way they are named, i.e in title case:  
```
<ChildComponent />
```
9. All vuex **getters** should be consumed **only** through the `computed` block, and then the computed (returned from getter) is consumed in the component. 
10. All formatting functions should sit in the **filters** block and not the **methods** block nor the computed block.
11. All functions should carry a single responsibility.
12. Logics tied to a child component should live in the child component and not in the parent.
13. Looped child component should carry a key with the unique loop id or a unique property where available else the index. e.g.  
	  This -> `<ProductCanvas v-for="(product, pIndex) in products" :key="product.id" :product="product" />`  
	  Not this -> `<ProductCanvas v-for="(product, pIndex) in products" :key="pIndex" :product="product" />`  
14. A value that depends on the calculation/logic of some data block values should always be placed in the `computed` block.
15. There should always be a line spacing between the instance blocks(data, methods, mounted etc) and also between functions in the methods block. 
