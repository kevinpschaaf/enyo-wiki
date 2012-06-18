# Platform-Specific Deployment

The instructions that follow assume that you have completed the development phase
of an Enyo-based application as well as the optimization phase (i.e., running the
`deploy` script and removing any unnecessary files from the project) and that you
are now ready to create a final product for deployment to your platform of choice.

## Deploying as a Mobile Application

### Deploying to iOS Using PhoneGap

1. Follow the instructions in the [PhoneGap Getting Started Guide](http://phonegap.com/start#ios-x4) to install PhoneGap and create a basic iOS PhoneGap app.

2. Now drop your Enyo-based app files into the `www` directory created in step 1.

3. Add the following meta tag to `index.html` for proper display on device:

    `<meta name="viewport"; content="width=device-width, initial-scale=1.0,
    maximum-scale=1.0, user-scalable=no" />`

From here you can follow the instructions in the [PhoneGap Getting Started Guide](http://phonegap.com/start#ios-x4) to deploy to the simulator or device.  To submit your app to the Apple App Store, you'll need to sign up for a developer account and review the documentation provided at <http://developer.apple.com>.

Finally, if your project requires access to PhoneGap's native functionality, follow the directions in [Making Use of PhoneGap’s Native Functions](PhoneGap-Native-Functions).

### Deploying to Android Using PhoneGap

1. Follow the instructions in the [PhoneGap Getting Started Guide](http://phonegap.com/start#android) to install PhoneGap and create a basic Android PhoneGap app.

2. Now drop your Enyo-based app files into the `www` directory created in step 1.

3. Add the following meta tag to `index.html` for proper display on device:

    `<meta name="viewport"; content="width=device-width, initial-scale=1.0,
    maximum-scale=1.0, user-scalable=no" />`

From here you can follow the instructions in the [PhoneGap Getting Started Guide](http://phonegap.com/start#android) to deploy to the emulator or device. To publish your app on Google Play, you'll need to sign up for a developer account and review the documentation provided at <http://developer.android.com>.

Finally, if your project requires access to PhoneGap's native functionality, follow the directions in [Making Use of PhoneGap’s Native Functions](PhoneGap-Native-Functions).

### Deploying as a Mobile App Using PhoneGap Build

1. Create a zip file of your project.

2. Upload the zip file into your PhoneGap Build application (follow the instructions on the [Web site](https://build.phonegap.com/)).

3. PhoneGap Build outputs application packages for various platforms.

## Deploying as a Google Chrome Application

1. Create a `manifest.json` file in your application's root directory, e.g.:

		{
			"name": "Testplate",
			"version": "1.0",
			"manifest_version": 1,
			"description": "Enyo extension.",
			"app": {
				"launch": {
					"local_path": "index.html"
				}
			}
		}

2. In Google Chrome, choose **Tools|Extensions**, then pick **Load Unpacked Extension** and select your deployment folder (e.g., `myapp-deploy`).

3. A generic application icon appears on your Chrome Apps page that will invoke this app.

4. Refer to the [Chrome documentation](http://code.google.com/chrome/extensions/apps.html) for more information on manifest files and the actual packaging of an application.

## Deploying as an Installable Windows Application

(**Note:** These instructions make use of the [Intel AppUp(TM) Encapsulator](http://appdeveloper.intel.com/en-us/encapsulator-beta).  We suggest that you use Encapsulator 1.0.)

1. Create a zip file of your project.  (**Note**: Make sure this zip contains only the project files and is not a zip of the project folder.)

2. Fill in the fields on the Encapsulator tool as you like.  When asked, choose the zip file from step 1.

3. Press the **Make It** button.

4. When the Encapsulator is done, it will produce a Windows setup file.  Run this file to install your application as a native Windows app.
