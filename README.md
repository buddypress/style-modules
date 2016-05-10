# Style Modules - BuddyPress Styling Snippets Library

----------------------------------------------------------------------------------------------


This repo maintains the community written style modules.

The Project proposal was written up and published to BuddyPress's bpdevel site:
(https://bpdevel.wordpress.com/2016/03/30/bp-style-modules-a-proposal/)

This repo contains one example  - but fully functioning  - module directory (bp-members-list) . The ultimate purpose of the repo is to maintain the modules as seen in the repo _not_ to clone the repo or download the repo (unless ones in initial core dev work, in which case by all means do clone the repo to obtain the dir structure and grunt tools) Module authors and users would be submitting and downloading the individual module folders and files.

Modules Listings:
Modules available for download are listed on our wiki:
<a href="https://github.com/buddypress/style-modules/wiki/Style-Modules-Listings">**Style Modules Listings**</a>

# BP Style Module authoring guidelines & installation instructions

_Style Modules are provided on the understanding that they are authored by members of the community, and while BP will undertake some basic checks on the files it cannot guarantee the suitability of a module in a given theme nor accept responsibility for updates or any general liability for the use of modules on a site.. Use of the modules is strictly at the users discretion._

This readme covers essential guidence for:
* **<a href="#authors-guidelines">Authors</a>**
* **<a href="#user-guidelines">Users</a>**

## <span id="author-guidelines">Authors Guidelines</span>:


###Submitting###
All submissions should be made by creating a ticket on [trac.buddypress] (https://buddypress.trac.wordpress.org/newticket) and selecting `Style Module` as the ticket type.

_Any questions or problems related to creating a module may be raised on the style module repos issue tracker as a ticket:
<a href="https://github.com/buddypress/style-modules/issues/new">New Ticket</a>_

Upload your folder(files) zipped complete with seperate SC & readme.md

Add a brief description of your submissions styling purpose e.g _"styles the user message tables..."_

The screen capture will provide a quick visual reference to your module styling, (label it style-module.jpg as we will attempt to load this image file on the modules listing page and in the readme.md file please add a link to the image - see the example readme)

A core BP developer  will download your submission and run a basic check for formatting standards and error free code, when two devs have signed off on the submission the zip and SC/readme files will be uploaded to the BuddyPress github repo where it will be available for people to download.

###Updates###

Authors are responsible for maintaining their modules against BP release cycles, ensuring that their module still performs as expected for new BP releases.

###Creating Module files:###

* All Modules must include one of either a .css or .js file
* Modules must also include a readme.txt file (follows the general format of WP plugin readme files for convenience and conformity to a known existing standard.
* Additionally each module should contain a readme.md file this is specifically for github display and allows us to display a neat and simplified module title and description to be displayed for users
* Optionally  please try to include a SC of your styles in action.

_You may use the members list module as a guide for readme formats_

**Grunt task tools**

The repo root does contain a package.json to install grunt modules via npm along with a configured gruntfile.js to load and configure tasks.

Feel free to use thse tools or not the choice is entirely yours.

The gruntfile sets up scss, less, scsslint, rtlcss, and watch tasks.

At the cli running:
$ grunt watch // will start the watch task, configured by default to watch for changes to .scss files in your named dir

$ grunt scsslint // will check your scss file against the yml config file in the repo root (these are BP's default file formatting)

$ grunt rtlcss // will create a right to left mirror file from your css file.

$ grunt commit // is a reg task that will run both linting and rtl tasks one after the other.

If you do use these tools you must configure the top var MODULE_NAME = 'my-cool-styles-module';

This var is used to build or the file paths and file names we need and in the correct directory so is important.

In addition specify what pre-processor type your using scss or less.

The less provision is mainly commented out and the less aspects would need testing but should work.

Naturally if you're using the gruntfile please feel free to re-factor as you feel fit, this is only provided as a convenience.

####readme.txt file####
we'll broadly follow the conventions of use by WordPress plugins for this file; for Modules the important areas are.
* The header details ( initially BP might be using these but we are adding so that we can at a later date)
* The module description - this is how we'll ensure that users can understand what the module offers ( SC also help here! )
* Installation guide complete with copy/paste code ( following the example provided we need  the module to provide quite detailed steps to installing, copy/paste code using your module name where applicable will help non technical users)

####The style & JS files####

These files will follow the formatting guidelines set out by WP.

**Stylesheets**
Formatting guidelines: [WP Coding Standards - CSS] (https://make.wordpress.org/core/handbook/best-practices/coding-standards/css/)

Your file should pay attention to using the BP id '#buddypress' to ensure your rules are targeted to BP elements only, also they should try and use ancestor elements that represent the component this module is dealing with.

You may work with scss or less however please ensure compiled files are present in the module package along with the source pre-processor file.

rtl versions - you will know whether you need to address rtl layouts (i.e floats left & right) if so please include rtl file version.

**JavaScript**
Formatting guidelines:  [WP Coding Standards - JS] (https://make.wordpress.org/core/handbook/best-practices/coding-standards/javascript/)

## Module Installation Instructions: ##
In your readme.txt file please include details on how to enqueue your files. Provide a copy and paste example for your particular module enqueued in the manner WP recommends. The example below will handle finding the BP dir name used  and build the paths, you would simply need to add the style modules name.
e.g:

		-------------------------------------------------------------------------------
		/*
		* Check & build the required paths to the files
		*/
		function sm_module_path() {

		$sm_parts = array();
		// Authors add your modules name
		$module_name = '';

		if( file_exists( get_stylesheet_directory() . '/buddypress/') ) :
		 $sm_dir = get_stylesheet_directory_uri() . '/buddypress/style-modules/';
		else:
		 $sm_dir = get_stylesheet_directory_uri() . '/community/style-modules/';
		endif;

		$sm_parts['sm_dir']       = $sm_dir;
		$sm_parts['path_to_file'] = $sm_dir .  $module_name . '/';
		$sm_parts['module_name']  = $module_name;

		return $sm_parts;
		}

		function enqueue_module_style() {
		 $sm_parts = sm_module_path();
		 $dir  = $sm_parts['path_to_file'];
		 $rtl = ( is_rtl() )? '-rtl' : '';
		 $file = $sm_parts['module_name'];
			wp_enqueue_style( $file . '-styles',  $dir . $file . $rtl . '.css', array('bp-legacy-css'), false, 'screen' );
		}
		function enqueue_module_script() {
		 $sm_parts = sm_module_path();
		 $dir  = $sm_parts['path_to_file'];
		 $file = $sm_parts['module_name'];
			wp_enqueue_script( $file . '-script', $dir . $file . '.js', array('jquery'), false, true );
		}
		add_action('bp_enqueue_scripts', 'enqueue_module_style', 20);
		add_action('bp_enqueue_scripts', 'enqueue_module_script');

		-------------------------------------------------------------------------------


## <span id="user-guidelines">User Guidelines</span>:

**Installing a chosen module:**

Users will need to create a folder in their`/buddypress/` or `/community/` directory to hold the style modules. Modules are always located in the folder `/style-modules/` so to use the `members-list-module`  you would need to create a folder structure that looks like this:
`wp-content/my-theme/buddypress/style-modules/members-list-module/`

You need to download the zip file - from the listing download link or directly from the github repo -  and copy the modules loading/enqueueing functions to your functions.php file.
