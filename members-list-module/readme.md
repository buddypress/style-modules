#BuddyPress Members List Style Module
![Members rendered as a series of floated box panels](https://github.com/hnla/style-modules/blob/master/members-list-module/members-as-grid-view.jpg "Styling the members lists as box grid.")
## Description:

This module styles your members listings in the members directory and lists in the users profile pages as box panels arranged in a grid style for medium sized devices like tablets and desktops in landscape orientation and at mobile widths stacks the lists vertically.

The module requires a companion script file providing some enhancements to add equal height to elements.

## Installing

To install this module copy it to your child theme. The module needs to live in a directory named `/style-modules/` this directory or folder lives in turn in the child themes `/buddypress` or `/community` directory depending on how you have named your BP directory. You should then have something similar to this structure:

`my-child-theme/buddypress/style-modules/members-list-module/`

To load the required files you will need to add this block of code to your functions.php file in your child theme:
(**copy & paste the code between the dashed lines**)

	-------------------------------------------------------------------------------

		/*
		* Check & build the required paths to the files
		*/
		function sm_module_parts() {

			$sm_parts = array();

			########## AUTHOR CONFIG ##########
			// Authors add your modules name & set whether your module has a JS file
			$module_name = 'members-list-module';
			// Does your module have a supporting JS file? ( bool true/false )
			$js_support = false;
			########## END AUTHOR CONFIG ##########

			if( file_exists( get_stylesheet_directory() . '/buddypress/') ) :
				$sm_dir = get_stylesheet_directory_uri() . '/buddypress/style-modules/';
			else:
				$sm_dir = get_stylesheet_directory_uri() . '/community/style-modules/';
			endif;

			$sm_parts['sm_dir']       = $sm_dir;
			$sm_parts['path_to_file'] = $sm_dir .  $module_name . '/';
			$sm_parts['module_name']  = $module_name;
			$sm_parts['js_support']   = $js_support;

			return (object) $sm_parts;
		}

		function enqueue_module_style() {
			$sm_parts = sm_module_parts();

			if( empty( $sm_parts->module_name ) )
			return false;

			$dir  = $sm_parts->path_to_file;
			$rtl  = ( is_rtl() )? '-rtl' : '';
			$file = $sm_parts->module_name;
			wp_enqueue_style( $file . '-styles',  $dir . $file . $rtl . '.css', array('bp-legacy-css'), false, 'screen' );
		}

		function enqueue_module_script() {
			$sm_parts = sm_module_parts();

			if( false === $sm_parts->js_support )
			return false;

			$dir  = $sm_parts->path_to_file;
			$file = $sm_parts->module_name;
			wp_enqueue_script( $file . '-script', $dir . $file . '.js', array('jquery'), false, true );
		}

		function sm_load_files() {
			if( !empty( sm_module_parts()->module_name ) ) {
				add_action('bp_enqueue_scripts', 'enqueue_module_style', 20);

				if( false !== sm_module_parts()->js_support )
					add_action('bp_enqueue_scripts', 'enqueue_module_script', 20);
			}
		}
		add_action('bp_after_setup_theme', 'sm_load_files');

	-------------------------------------------------------------------------------

If you want to load more than one module then you might find it easier to use the module loader plugin which will look for all modules and load all the files it finds automatically:
{link_to_loader_file}
