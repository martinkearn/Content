
# Visual Studio 2017 Update 3 for Web Devs
A tour around new features in Visual Studio for web developers

### Pre-reqs
* Visual Studio 2017 with update 3 and [.net Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd)
* [ASP.NET Core Template Pack 2017.3](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.ASPNETCoreTemplatePack20173)
* [Error Catcher II](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.ErrorCatcherII)
* [Surface Dial Tools](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.SurfaceDialToolsforVisualStudio)
* [Web Accessibility Checker](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.WebAccessibilityChecker)
* Google Chrome with [Web Essentials Chrome Extension](https://chrome.google.com/webstore/detail/web-essentials/mghdcdlpcdiodelbplncnodiiadljhhk)
* Ideally have a ASP.NET Core Web Application (MVC) project pre-created for the HTML Accessibility Checker step onwards

## ASP.NET Core Template Pack 2017.3 
File > New Project > ASP.NET Core Web Application

Talk through each template

Static Web
* ASP.net core project with just JS, HTML and CSS
* AspNetCore.StaticSiteHelper Nuget package - gives ASP.net tooling without an ASP.Net content

MVC Basic
* Very simple, streamlined MVC template based around an employee model
* No bootstrap, jquery or anything like that

MVC High Performance
* Like MVC basic
* Tuned for ultimate web performance

## SPA Templates
New with .net Core 2.0 we now have SPA templates

File > New Project > Angular (also available via the CLI). may need to close and re-open VS for it to build

Highlight new `clientapp` folder

Show `UseWebpackDevMiddleware` in `startup.cs`

Run app in browser and explore it (counter, data)

Make a change to header in `clientApp/app/components/counter/counter.component.html` and see the browser update on save

Edit `clientApp/app/components/counter/counter.component.ts` to be
		
```
export class CounterComponent {
    public currentCount = 1;

    public incrementCounter() {
        this.currentCount = this.currentCount +100;
    }
}
```

## HTML Accessibility Checker
Create a 'web application mvc' project

Run in Chrome (with Web Essentials extension)

Show warning about html lang attribute in Visual Studio

Add the `lang="en"` attribute to the html doc and see the warning go away

Change `footer .container` `colour` to `#999999` and notice a warning shows up in VS

Make darker with Surface Dial
* Hold click dial > select editor
* Move dial anti-clockwise to make colour darker

Save and see browser update and error go away

## JavaScript Intellisense
Carry on with the 'web application mvc' template from before

In `site.js`, type `(function` and tab to complete the code
* Tidy up to look like this
```
(function () {

})();
```
			
Add this code
```
hello("hello");

function hello(greeting) {
}
```

Type `const elm = document.qs` (qs will intellisense to query selector) > select h1 (`document.querySelector("h1");`)

Type `const text = '${greeting} world@'` (' is the character that signifies code in markdown)

Type `elm.` > choose innerHTML (it knows this is a html element)

Open Index and add a `<h1></h1>`

Run

Put VS and browser side-by-side

P element says "hello world"

## Live JavaScript Editing
Change `hello("hello");` to `hello("Waynes");`

Save

Browser will refresh

## ESLint and Error Catcher Extension
Go to `site.js`

Show ESLint warning glyph in VS about missing semi colon (provided by the Error Catcher II extension)

Add semi colon > save > error goes away
* The glyph and the warning window

## Surface dial debugging in Chrome
_We now have client side debugging between VS and chrome - been in IE for many years but now in chrome_

Make sure chrome is set to current browser

In `site.js` put a breakpoint on `hello("Waynes");`

_Surface Dial is a bluetooth accessory (works with any Windows PC) that does tonnes of great things in Windows. It is also supported in VS via the Surface Dial Tools extension_

Click and hold on Surface Dial to bring up menu > Debug

Click Surface Dial to run application

Notice breakpoint

Click to step into

Rotate right to step over

Show Locals window

Edit value of `const text` to say "Hello WHW"

F5 to complete