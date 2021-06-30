# Nuxt(Vue) Best Practices
The Vue lifecycle comes in handy and increases frontend development ETA, Nuxt comes along with special features as an addition to this lifecycle, these special features are powered by the `nuxt.config.js` and this can immediately get opinionated and complex to scale.

## Installation 
1. `yarn` is the package manager of choice.
2. Every project **must** start with the create nuxt app command:  
`yarn create nuxt-app {project-name or (./)[i.e. current folder]}`
3. From **2. above**, here are the answers to the interactive in compliance to our frontend projects:  
	`Project name:` {**name of folder by default}** This should always have the format `{projectname-customer-facing}`  
	`Programming language:` {**javascript**}  This is the preferred option.  
	`Package manager:` {**yarn**} This is the preferred option.  
	`UI framework:` {**Tailwind CSS**} Utility first and it's none unintrusive to the webflow exports and other primary css framework used. Helps with quick space adjustments and etc.  
	`Nuxt.js modules:` {**Axios** and **PWA**} These two are the select choices.  
	`Linting tools:` {**Eslint**} This is the only select choice. Prettier should never be used.  
	`Testing framework:` {optional but if required **Jest**} This is preferred choice.  
	`Rendering mode:` {**Single Page App**} SSR is cool but extends development time due to extra lines of checks for client & server side also it is expressly not required if proper care is taken while writing a SEO friendly SPAs  
	`Deployment target:` {**Static (Static/Jamstack hosting)**} This works well with Netlify.com our preferred CI/CD platform.  
	`Development tools:` {**jsconfig.json**} This is need for the editor. To void cases where advance directories (path) are not easily read especially for javascript.  
	`Continuous integration:` {**none**} No CI should be selected as Netlify will take care of all that.  
	`Version control system:` {**Git**}  

## Setup
1. The `.env` file should be the only file that has the environment variables.  
2. All functions in the methods block must be in **camelCase**.  
3. All functions must be prefixed with an action word e.g **fetchProducts**, **storeProduct**  
4. All functions responsible for HTTP request call should carry the same name as the action function in the `store/index.js`  
5. The plugin folder should contain the following files where applicable:  
	- prototypes.js  
	- filters.js  
	- components.js  
	- directives.js  
	- 
