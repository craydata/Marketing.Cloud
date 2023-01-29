# SFMC.BestPractices
## Folder Structure

```
project-root/
  cloudPages/
    cloudPageName1/
      app/ (this is where all front end code goes)
      lib/
        jquery.min.js
        bootstrap.min.css
         ... (optional; libraries that you want to host in SFMC)
      scss/
        ... (optional; all your SASS files)
      js/
        ... (all your non-minified JS files)
        app-1.html
        app-2.loadExternalLibs.html
        app-3.style.min.css
        app-4.script.min.js
      server/ (this is where SSJS and AMPscript goes)
        lib/
        ... (any SSJS files containing shared classes / polyfills, ...)
        server-1.amp
        server-2.initCore.ssjs
        server-3.ssjs
  cloudPageName2/
...
  emails/
...
contentBlocks/ (sometimes you will want to prepare code snippets)
  snippet1/
...
```
