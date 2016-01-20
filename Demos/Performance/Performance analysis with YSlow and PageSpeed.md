
# Performance analysis with YSlow and PageSpeed
A demo that shows how to use the YSlow and PageSpeed tools

### Pre-reqs
* Chrome with YSlow plug-in
* Ensire CDNs are added in YSlow
* Ensure web dev checklist add-in is installed

## Open Chrome
Open chrome to a website
* Ask the audience
* http://www.lingscars.com/
* http://www.microsoft.com/en-gb/
	
## Web Dev CheckList
Use add-in to show quick view

Open the 'more info' site

Show the ASP.net section

Show the ASP.net > Performance section

## YSlow
Use the add-in to run a test

Look at overall score / grade

Look at bad areas
* F Add expires headers
* F Reduce the number of DOM elements
* E Do Not scale images in HTML

Look at good areas
* A Put CSS at top
* B Put javascript at bottom
* A minify JavaScript and CSS

Components

Statistics
* Empty vs primed cache

## Google Page Speed
In Chrome https://developers.google.com/speed/pagespeed/insights/

Enter url and analyse

Examine suggestions
* Optimize images
* Enable compression
* Eliminate render-blocking JavaScript and CSS in above-the-fold content

Show Mobile and Desktop tabs

## Babble Kids
http://babblekids.co.uk
* Full ecommerce ASP.net MVC 5 Database driven via EF on shared Azure instance
* Images, Responsive (big DOM), JavaScript
* Google, Clicky and Azure analytics

Run YSlow

Grade
* A, 99/100
* Google analytics causes 99 instead of 100 because of its frequent cache expiration
* Started at 73 and spent 4 hours on it

Components
* 2 JS, 1 CSS (through bundling)

Stats
* Only 2 requests / 5.9k on primed cache