# HTML_CodeSniffer README

## What is HTML_CodeSniffer?

HTML_CodeSniffer is a JavaScript application that checks a HTML document
or source code, and detects violations of a defined presentation or accessibility
standard.

### Standards included

By default, HTML\_CodeSniffer comes with standards that cover the three conformance
levels of the <abbr title="World Wide Web Consortium">W3C</abbr> [Web Content Accessibility Guidelines (WCAG) 2.0](http://www.w3.org/TR/WCAG20),
and the <abbr title="United States of America">U.S.</abbr> [Section 508](http://section508.gov/index.cfm?fuseAction=stdsdoc) legislation.
It also provides tools to write your own standards, which can be useful in situations
where you wish to enforce consistency across a web site.

### Using HTML_CodeSniffer

HTML\_CodeSniffer can be called in multiple ways:
* Called directly in JavaScript source, HTML_CodeSniffer will provide a list of known
  and potential violations to the calling script.
* It also comes with a pop-up auditor interface, accessible via a bookmarklet,
  letting you browse through messages emitted from one of the defined standards. 
  Where possible, the auditor also points you to the HTML element causing the problem.
* It can also be run on the command line with the assistance of a headless browser app.
* Using npm:
```
$ npm i -g npm
$ npm i --save html_codesniffer
```

### Licence

HTML_CodeSniffer is released under a BSD-style licence. For more information,
please see the file "licence.txt".

## Using the source code

### Building the auditor

The HTML\_CodeSniffer auditor can be built using [node.js](https://nodejs.org/) and the Grunt
tasker (http://gruntjs.com/). It has been tested with the latest version of node.js
(at time of writing: version 6.0) and Grunt, but should also work with recent
earlier versions. 

* Install node.js with your package manager of choice.
* You may need to update the Node.js package manager (npm) itself: 
  <code>npm update -g npm</code>
* Install the Grunt CLI helper if you haven't already done so:  
  <code>npm install -g grunt-cli</code>
* Get node.js to install the dependencies Grunt needs:  
  <code>npm install</code>
* Run Grunt to build the auditor:
  <code>grunt build</code>
  
You should see two new directories: <code>node_modules</code> (containing the node.js
dependencies), and <code>build</code> (containing your auditor). You can then move
(or symlink as appropriate) your <code>build</code> directory to a web-accessible
location.

Then grab or copy the JavaScript from the auditor bookmarklet from the [HTML_CodeSniffer site](https://squizlabs.github.io/HTML_CodeSniffer),
replace the directory at the start (//squizlabs.github.io/HTML_CodeSniffer/build) with your local URL, and save as a new bookmarklet.

#### Debug build

If you are developing using HTML\_CodeSniffer and require the code not minified for
debugging purposes, follow the above steps, but run <code>grunt build-debug</code>
(instead of just build). This will combine the files as normal, but not minify them.
  
### Command-Line processing

**Note:** These examples assume a built version of HTMLCS exported to `./build/HTMLCS.js`

#### PhantomJS

You will need [PhantomJS](http://www.phantomjs.org/) installed if you wish to
use the contributed command-line script. PhantomJS provides a headless Webkit-based
browser to run the scripts in, so it should provide results that are similar to 
recent (or slightly less than recent) versions of Safari.

See the <code>Contrib/PhantomJS/HTMLCS_Run.js</code> file for more information.

#### Node & JSDom

HTML_CodeSniffer requires a dom to run, however, it is possible to run it entirely
server side without a headless browser using Node on arbitrary fragments of HTML using
an environment wrapper like [JSDom](https://github.com/jsdom/jsdom).

An example node script:
```javascript
var jsdom = require('jsdom');
var { JSDOM } = jsdom;
var fs = require('fs');

var HTMLCS = fs.readFileSync('./build/HTMLCS.js', 'utf-8');
var vConsole = new jsdom.VirtualConsole();

// Forward messages to the console.
vConsole.on('log', function(message) {
    console.log(message)
});

var dom = new JSDOM('<img src="test.png" />', {
    runScripts: "dangerously",
    virtualConsole: vConsole
});

dom.window.eval(HTMLCS);
dom.window.HTMLCS_RUNNER.run('WCAG2AA');
```

### Translations

HTML_CodeSniffer supports _very_ basic string translations. The auditor will use the current language of the document it is being run in (e.g. `<html lang="en">`). A language code can be supplied if you need to tell HTML_CodeSniffer which language you want to use.

Example usage:
```javascript
HTMLCSAuditor.run('WCAG2AA', null, {
  lang: 'pl'
});
```

**Note:** HTML_CodeSniffer only has english (default) and polish language.

If other language support is required a custom version can be built by adding more translations in `Translations/<code>.js` and using the grunt build process described above.

### Contributing and reporting issues

To report any issues with using HTML_CodeSniffer, please use the
[HTML_CodeSniffer Issue Tracker](http://github.com/squizlabs/HTML_CodeSniffer/issues).

Contributions to the HTML_CodeSniffer code base are also welcome: please create a
fork of the main repository, then submit your modified code through a
[Pull Request](http://help.github.com/send-pull-requests/) for review. A Pull Request
also automatically creates an issue in the Issue Tracker, so if you have code to
contribute, you do not need to do both.

## More Information

More information on HTML_CodeSniffer can be found on its GitHub site,
[http://squizlabs.github.io/HTML_CodeSniffer/](http://squizlabs.github.io/HTML_CodeSniffer/). This site provides:

- Information on the tests performed (and messages emitted) by HTML_CodeSniffer's standards, organised by conformance level and Success Criterion;
- A source test area that allows you to try out HTML_CodeSniffer with your own HTML source code; and
- A link to the HTML_CodeSniffer bookmarklet, letting you check other pages using the pop-up auditor interface.
