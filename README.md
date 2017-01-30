<!-- Library Logo -->
<img src="https://github.com/jrvansuita/PickImage/blob/master/app/src/main/res/mipmap-xxxhdpi/ic_launcher.png?raw=true" align="left" hspace="1" vspace="1">

<!-- Buy me a cup of coffe --> 
<a href='https://ko-fi.com/A406JCM' target='_blank' align="right"><img  align="right" height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi4.png?v=f' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
# PickImage

This is an [**Android**](https://developer.android.com) project. It shows a [DialogFragment](https://developer.android.com/reference/android/app/DialogFragment.html) with Camera or Gallery options. The user can choose from which provider wants to pick an image.

</br> 
</br> 

<!-- JitPack integration -->
[![](https://jitpack.io/v/jrvansuita/PickImage.svg)](https://jitpack.io/#jrvansuita/PickImage)<!-- Android Arsenal -->
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-PickImage-green.svg?)](https://android-arsenal.com/details/1/4614)<!-- License -->
<a target="_blank" href="/LICENSE.txt"><img src="http://img.shields.io/:License-MIT-yellow.svg" alt="MIT License" /></a><!-- Minimun Android Api -->
<a target="_blank" href="https://developer.android.com/reference/android/os/Build.VERSION_CODES.html#GINGERBREAD"><img src="https://img.shields.io/badge/API-9%2B-blue.svg?style=flat" alt="API" /></a><!-- Methods Count -->
<a target="_blank" href="http://www.methodscount.com/?lib=com.github.jrvansuita%3APickImage%3Av2.0.2"><img src="https://img.shields.io/badge/Methods-409-e91e63.svg" /></a><!-- Hits Count -->
[![ghit.me](https://ghit.me/badge.svg?repo=jrvansuita/PickImage)](https://ghit.me/repo/jrvansuita/PickImage)<!--Open Source --> [![Open Source Love](https://badges.frapsoft.com/os/v2/open-source.svg?v=103)](https://github.com/ellerbrock/open-source-badges/)


# Screenshots
<img src="images/dialogs/light_vertical_left_simple.png" height='auto' width='280'/>
<img src="images/dialogs/light_horizontal_top_simple.png" height='auto' width='280'/>
<img src="images/dialogs/light_horizontal_left_simple.png" height='auto' width='280'/>
<img src="images/dialogs/light_vertical_left_colored.png" height='auto' width='280'/>
<img src="images/dialogs/light_horizontal_top_colored.png" height='auto' width='280'/>
<img src="images/dialogs/light_horizontal_left_colored.png" height='auto' width='280'/>
<img src="images/dialogs/dark_vertical_left.png" height='auto' width='280'/>
<img src="images/dialogs/dark_horizontal_top.png" height='auto' width='280'/>
<img src="images/dialogs/dark_horizontal_left.png" height='auto' width='280'/>



# Setup

#### Step #1. Add the JitPack repository to your build file:

    allprojects {
		repositories {
			...
			maven { url "https://jitpack.io" }
		}
	}

#### Step #2. Add the dependency

    dependencies {
           compile 'com.github.jrvansuita:PickImage:v2.0.2'
	}

# Implementation

#### Step #1. Overriding the library file provider authority to avoid installation conflicts. 
######_The use of this library can cause [INSTALL_FAILED_CONFLICTING_PROVIDER](https://developer.android.com/guide/topics/manifest/provider-element.html#auth) if you skip this step. Update your manifest.xml with this provider declaration._

    <manifest ...>
        ... 
        <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="${applicationId}.com.vansuita.pickimage.provider"
            tools:replace="android:authorities">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/provider_paths" />
        </provider>
    </manifest> 
    
#### Step #2 - Showing the dialog.
######_It's absolutely necessary to give to [PickSetup](/library/src/main/java/com/vansuita/pickimage/PickSetup.java) constructor your application id._

    PickImageDialog.on(MainActivity.this, new PickSetup(BuildConfig.APPLICATION_ID));
    
    //or 
    
    PickImageDialog.on(getSupportFragmentManager(), new PickSetup(BuildConfig.APPLICATION_ID));
    
#### Step #3 - Applying the listeners.

##### Method #3.1 - Make your AppCompatActivity implements IPickResult.

    @Override
        public void onPickResult(PickResult r) {
            if (r.getError() == null) {
                ImageView imageView = ((ImageView) findViewById(R.id.result_image));
    
                //If you want the Bitmap.
                imageView.setImageBitmap(r.getBitmap());
    
                //If you want the Uri.
                //Mandatory to refresh image from Uri.
                imageView.setImageURI(null);
    
                //Setting the real returned image.
                imageView.setImageURI(r.getUri());
                
                //Image path.
                r.getPath();
            } else {
                //Handle possible errors
                //TODO: do what you have to do with r.getError();
            }
        }
           
##### Method #3.2 - Set the listener using the public method.

    PickImageDialog.on(getSupportFragmentManager(), new PickSetup(BuildConfig.APPLICATION_ID))
                   .setOnPickResult(new IPickResult() {
                      @Override
                      public void onPickResult(PickResult r) {
                         //TODO: do what you have to...
                      }
                });


#### Step #4 - Customize you Dialog using PickSetup.
    PickSetup setup = new PickSetup();
    setup.setBackgroundColor(yourColor);
    setup.setTitle(yourTitle);
    setup.setDimAmount(yourFloat);
    setup.setTitleColor(yourColor);
    setup.setFlip(true);
    setup.setCancelText("Test");
    setup.setImageSize(500);
    setup.setPickTypes(EPickTypes.GALERY, EPickTypes.CAMERA);
    setup.setProgressText("Loading...");
    setup.setProgressTextColor(Color.BLUE);

# Samples
 You can take a look at the sample app [located on this project](/app/).

# Additionals

#### Own click implementations.
###### _If you want to write your own button click event, your class have to implements [IPickClick](library/src/main/java/com/vansuita/pickimage/listeners/IPickClick.java) like in the example below. You may want to take a look at the sample app._
 
     @Override
     public void onGalleryClick() {
         //TODO: Your onw implementation
     }
 
     @Override
     public void onCameraClick() {
         //TODO: Your onw implementation
     }
     
#### For dismissing the dialog.
     PickImageDialog dialog = PickImageDialog.on(...);
     dialog.dismiss();
     
