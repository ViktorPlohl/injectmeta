# A command line utility to inject meta tags into an html file or to remove code based on conditions.
Written because I could not find a command-line equivalent that could work with npm scripts instead of gulp.

This module injects meta tags between markers placed in an html file or removes code between markers.
It can take a json file which will be transformed to strings and injected.
It removes code between markers so you can for example remove livereload code which is not needed in a production environment.
It can also inject the current git commit hash into the HTML as a comment so it is easily visible which commit is currently 
deployed.

**This module was written in ES6 and requires node >= 4.0.0.**

## Usage
Place the markers in your html file to inject or remove code:

```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>Home</title>
     <meta name="viewport" content="width=device-width, initial-scale=1">

     <!-- inject:meta -->
     <meta name="devmeta" content="This meta will be replaced">
     <!-- endinject -->

   </head>
   <body>
        content
   </body>
   </html>
   <!-- inject:git-hash -->
```
The meta tags will be injected between the markers and the meta that are already there will be removed.

 Options:

    -h, --help             output usage information
    -V, --version          output the version number
    -i, --input <input>    Input file
    -o, --output <output>  Output file (defaults to input when omitted)
    -r, --remove <remove>  Remove condition
    -H, --hash             Inject git hash of current commit

### Examples

Given a meta.json file:

```json
{
	"meta": [{
			"name": "testmeta",
			"content": "Example content"
		},{
			"name": "testmeta2",
			"content": "Example content 2"
		}]
}
```
`injectmeta -i index.html -o dist/index.html -m meta.json`

will result in the file `dist/index.html`:

```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>Home</title>

        <meta name="testmeta" content="Example content">
        <meta name="testmeta2" content="Example content 2">

   </head>
   <body>

   </body>
   </html>
   <!-- b64f022ae51d87a411c4608f403f3216ee028d03 -->
```

*Notice that the code*
```html
     <!-- remove:production -->
     <script src="http://localhost:35729/livereload.js?snipver=1"></script>
     <!-- endremove -->
```
*has been removed.*

*The comment*
```html
    <!-- inject:git-hash -->
```
*has been replaced by the git hash of the current commit.*

