# Publish Aurora articles to Drupal
*current version v0.1*

The code provided in the wwAuroraDrupal.php file is a way to let Drupal receive published articles from WoodWing Inception or Aurora. 
In this documentation where you read Aurora you can also read Inception.

With this file installed and configured, you will be able to publish articles in two ways:

1) **As an Iframe:** This will show the article exactly as it was designed in Aurora. This option does not require any additional coding to make it work.

2) **As native Drupal:** This will show the article as a native Drupal post. Due to all the different possible modules in Drupal, this code path will require additional programming to make it work 100%

See also the discusion later on about  [who is in control](#choosing-the-display-mode-of-published-articles)


## Prerequisites

### Documentation
Knowledge of WoodWing Enterprise and the Aurora publish process is advised.
This can be found at the helpcenter: 

[Setup channels for Inception](https://helpcenter.woodwing.com/hc/en-us/articles/205571815-Setting-up-a-custom-Publish-Channel-in-Inception)

[Partner Enablement session](https://helpcenter.woodwing.com/hc/en-us/articles/115005474886-2017-11-09-Enterprise-Aurora-Publish-Channels)


### Drupal modules

Two Drupal modules needs to be installed.

The iframe module will allow you to view the published article as it was designed in Aurora


Download from :

``-  https://www.drupal.org/project/iframe``


### PHP dependencies
To be able to unzip the package that will be downloaded, the php-zip module should be active within PHP configuration. 

## Installation 
Place the ``wwAuroraDrupal.php `` in the root of the Drupal folder.



## Configuration
The configuration needs to be done in the file ``wwAuroraDrupal.php 

##### (optional) URL to SubServer
This is only required in this specific case:

If your Drupal server is not reachable from the outside world then the Aurora publish process can not push the article-packet to your WP.
To work around this, a 2-step process can be setup using a SubServer.
In this case the SubServer is running in the cloud and will receive the published articles. 
Your local WP can then 'pull' the published articles from this SubServer.

To make this work, the URL to the SubServer needs to be specified.

To 'pull' the content, the wwinception.php needs to be called from crontab on a regular interval.

## Run Config test
The configuration can be tested by invoking the script with the 'testconfig' parameter. Type the url like below in your browser,make sure to replace <server> with your server name or IP-address

``http://<server>/Drupal/wwAuroraDrupal.php?testconfig=1``

this should return the following information in your browser:

   

    WW-inception connector
	----------------------
	Checking setup
	- Check temp folder...OK

	- Check Log folder...OK

	Check SubServer...
	- Setting for SUBSERVER (AWSSNSURL) is empty, this 	configuration can only receive push from Inception

	Checking Drupal-plugin for Iframe...
	OK



if you do not see a similar message when you call the URL or when you receive a 404 error this indicates something is not working.

Please check if you have access on your webserver that allows you to run 'other' files (besides index.php) from the root of your Drupal installation. This is mostly handled in the .htaccess file.

***Before you continue***, make sure the 'testconfig' returns with the information above.



## Choosing the Display mode of published articles
Who is in Control? 

When publishing Aurora articles to a webCMS you have a decision to make.

The idea of Aurora is that the writer has full control about how the published article will look. But many webCMS systems are much more structured and will have custom 'widgets' to display for example images as slideshow or to embed video or adverts. So there is both a mismatch in concept (writer has design control versus webCMS has design control) and a mismatch in structure (writer decides on used widgets/components versus webCMS determines possible widgets/components)

with other words, you have to determine who should be in control, the writer or the webCMS?

### Writer in Control

In case you put the writer in control of the published article, then we will display the article in Drupal as it was designed in Aurora. This is done by using the iframe plugin. The complete Aurora article will be rendered in the iframe, using all styling, widgets and javascript from Aurora.

This is a solution that will give a predictable result 'out-of-the-box'

The disadvantage of the iframe method is that the article is shown as designed in Aurora and the used styling can be totally different then the active Drupal thema.

For real live implementations it is offcourse possible to create an Aurora styling that mimics the look and feel of the WebCMS. Allthough this also can be seen as branding.

### WebCMS in Control
In case you want to put the webCMS in control of the look and feel of the published articles then you will need to solve several technical challenges.

- mapping article components to cms-content structure
- mapping inception widgets to cms widgets
- uploading images to the correct WP structure
- uploading videos to the correct WP structure
- translate element/component names to WP-styling names

Based on the above (possible incomplete) list you can understand that this solution will not be available out-of-the-box. This solution will always be a fine-tuned connection between the possibilities of the CMS, custom styling in Aurora and also some PHP coding to make mapping/conversions.

Allthough work needs to be done to have a complete working setup without using the iframe, it is already possible to publish also in this 'mode'. The text and normal images will appear. Slideshow and more complex images (hero) will not display correctly. 


# Setup the custom channel

As this Drupal integration can be used by both Inception and Drupal we will give highlevel guidelines only. Detailed instructions can be found in the Woodwing 	helpcenter.


## Inception: Create the Custom channel
To be able to publish articles to our Drupal CMS we need to setup a so called 'Custom Channel' in the Inception configuration.

This setup is described in the Woodwing helpcenter

[How to setup a custom channel in Inception](https://helpcenter.woodwing.com/hc/en-us/articles/205571815-Setting-up-a-custom-Publish-Channel-in-Inception)

The url you need to specify as the end-point is depending on the location of your Drupal

Assume your Drupal is located at ``http://MyOwnDrupal``

Then to make the publish work as native Drupal post, you will need to specify the endpoint as:

``http://<server>/Drupal/wwAuroraDrupal.php``

and if you want to run in 'iframe' mode

``http://<server>/Drupal/wwAuroraDrupal.php?iframe=1``

If the Inception server can connect to your specified end-point, then there should be a green 'check' to indicate the end-point is valid
 

## Aurora: Create the Custom channel
To be able to publish articles to our Drupal CMS we need to setup a so called 'Custom Channel' in Enterprise ContentStation-Admin.

this setup is similar to that of Inception, but you need to login to the Enterprise Admin. Then select from the main menu 'Integrations'

You should see a orange 'ContentStation 10' icon. If you click that, the ContentStation 10 Management console will be openend.

On the left, you can select 'Publication Channels'

then in the drop-down in the top-middle of the screen you can select 'Custom Channel'

You can use the same endpoint defintion as for Inception:

``http://<server>/Drupal/wwAuroraDrupal.php?iframe=1``




