# UI Development Best Practices
Writing HTML/CSS or working with site building like Webflow, starts out clean and simple but within a short period into the project, the code usually gets overwhelming, if care is not taken. These are some of the best practices to ensure the large code-base is both readable, usable and scalable. 

---
 1. All header tags (`h1`-`h6`) should be styled directly (i.e not with classes) before commencing the development.
 2. All headers should be used semantically, i.e:
	 `h1` should always come first and should be the only one in a page, it should be followed by `h2`. Next level should be `h3` and not `h4` etc.
 3. All button tag text should be an action word (e.g: Submit, Buy, Login etc.) not a regular word (e.g: Feedback, Career, etc.)
 4. Class names should not be abbreviated. e.g. .`cta`(call to action), .`nlb`(nav link button) etc.
 5. Class names should be short and inline with the property or entity to which they  style. e.g: `.full-height`, `.display-show`, `.display-none`, `.opacity-0`, `.opacity-100`
 6. Classes should follow the utility first approach, they should be reusable and combinable. e.g:  
		`.section` -> adds padding and margin around an element  
		`.full-height` -> minimum height of 100vh  
		`.button` -> just all the properties a button should have.  
		`.text-blue` -> foreground color blue  
		`.bg-red` -> background color red  
end result should be **combinabilities** -> `<el class="section full-height bg-red"></el>`  
7. A major part of the interactions should be done with classes not inline styles, only edge interactions should be done with webflow (javascript). e.g:  
		`class="cart-icon hide"` -> element is hidden  
		`class="cart-icon show"` -> element is visible  
		`class="section collapsed"` -> element is collapsed  
		`class="section expanded"` -> element is expanded  
8. A documentation of all the interactions (with screenshots) should be done immediately after development is done/approved on webflow.
9. Any new update on UI after exported, should be directed to the UI developer, which is done strictly on webflow and re-exported, no UI update should be done expressly by the frontend developer.
10. Use of Lottie, should be better managed and images should be hosted on Cloudinary.com or an external image management tool.
11. Avoid inline styles
12. Avoid inline javascripts, let the frontend handle that. 
13. In the documentation of the interaction in 8. above should include all the external libraries used or proposed e.g GSAP, Lottie, Swiper etc.
