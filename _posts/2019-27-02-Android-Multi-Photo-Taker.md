---
title: "Android Multiple Photo Taker"
excerpt_separator: "<!--more-->"
description: "Android source code to add gallery feature to your project. (by Ayhan Avcı)"
date: "2019-02-28"
excerpt: "A native Android fragment that adds a custom gallery feature into your project. It can add, save, delete, preview, navigate photos and sends the file uri list to the caller."
header:
    og_image: /assets/images/simplicity1.png
categories:
  - Projects
tags:
  - Android
  - Java
  - Library
  - Multimedia

---
{% include toc title="Contents" icon="list-ul" %}

## Introduction

This is an Android fragment that adds a custom, mini gallery feature into your project. The source code is provided and sufficiently lean. The modification of the code and the integration to your project can be done easily.

## Features

* Takes photos one by one.
* Auto saves photos in full resolution into disk with unique ids.
* Resamples taken image bitmaps into lower quality bitmaps to reduce memory consumption and preview them all in one screen.
* User can scroll through previews and select any one of them for a larger preview.
* Can delete saved photos. Deleted photos are removed from disk, large & small previews.
* On Submit & Cancel, sends events to parent with photo uri list.
* On start, can load saved photos into previews using a uri list.
* No external dependencies, just native android sdk.
* Can simply be copy-pasted in any project.
* The preview image buttons are added dynamically and as it is, there is no limit to it. (You may want to edit the Java code to limit button creation according to your needs)

## Usage

The images used in layout file are stock Android vectors. I didn't add them here since it would be pointless. You may just use whatever icons you have.

* Add the layout & java files in your project. ([MultiplePhotoTakerFragment.java](https://github.com/ayhanavci/Android-MultiPhotoTaker/blob/master/MultiplePhotoTakerFragment.java) & [multiple_photo_taker_fragment.xml](https://github.com/ayhanavci/Android-MultiPhotoTaker/blob/master/multiple_photo_taker_fragment.xml))
* Add your own package name on top of the java file.
* Edit your AndroidManifest.xml for Camera & Storage permissions. Don't forget to add a FileProvider too. Check Android documentation on it. But it looks something like below.

```xml
  <manifest
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.CAMERA" />
    ...
    ...
    <application
      ...  
        <provider
          android:name="android.support.v4.content.FileProvider"
          android:authorities="${applicationId}.provider"
          android:exported="false"
          android:grantUriPermissions="true">
          <meta-data
            android:name="android.support.FILE_PROVIDER_PATHS"
            android:resource="@xml/file_paths" />
          </provider>
     ...
    </application>
  ...
  ...
```

* Load the fragment.

Here is an example:

{% highlight java %}
  ArrayList<Uri> photo_files = new ArrayList<>();
  //..
  //You can fill photo_files with uri list to previously saved images. This is passed on "newInstance". 
  //Below, same variable is also used to get submitted photos, which obviously you don't need to.
  //..
  MultiplePhotoTakerFragment multiplePhotoTakerFragment = MultiplePhotoTakerFragment.newInstance(photo_files);
  multiplePhotoTakerFragment.setUserActionEventsListener(new MultiplePhotoTakerFragment.IUserActionEvents() {
    @Override
    public void onSubmit(ArrayList<Uri> photo_file_paths) {
      //User clicked submit
      photo_files = photo_file_paths; //Save submitted photo uri list.
    }

    @Override
    public void onCancel() {
      //User clicked cancel
    }
  });
  FragmentTransaction ft = getSupportFragmentManager().beginTransaction();
  ft.replace(R.id.content_frame, multiplePhotoTakerFragment);
  ft.addToBackStack(null);
  ft.commit();
{% endhighlight %}

## Screenshots

1. Fragment starts

![1]({{ site.url }}{{ site.baseurl }}/assets/images/phototaker/1.png){:height="480px" width="256px"}

1. User clicks the photo+ icon. Camera intent starts, user takes a photo

![1]({{ site.url }}{{ site.baseurl }}/assets/images/phototaker/2.png){:height="480px" width="256px"}

1. Back to fragment screen. One photo taken

![1]({{ site.url }}{{ site.baseurl }}/assets/images/phototaker/3.png){:height="480px" width="256px"}

1. User keeps taking photos. The buttons are dynamic and there is no hard limit to it. (The limit is the memory!)

* User can click the red trash icon on top right to delete the previewed photo.
* Scroll left and right on the nested scroll with the small previews.
* Take more photos using camera+ icon.
* Click submit to send file list to onSubmit event or can cancel.

![1]({{ site.url }}{{ site.baseurl }}/assets/images/phototaker/4.png){:height="480px" width="256px"}

## MultiplePhotoTakerFragment Class

| Function | What it does  |
| ---------- | -------------------- |
| setUserActionEventsListener | The calling activity sets its event handler IUserActionEvents  |
| setUserActionEventsListener | The calling activity sets its event handler IUserActionEvents  |
| onCreateView | Framework override. Sets up button event handlers and loads photos (if any) into previews  |
| onRequestPermissionsResult | Framework override. Only called on first start, called after user allows camera usage. |
| loadSavedButtons | If parent activity passed a URI list, load them into previews. |
| onClickCancel | User clicked Cancel. Notify the parent and press back |
| onClickSubmit | User clicked Submit. Notify the parent with taken photo URI list and press back |
| onClickDeletePhoto | User clicked red trash icon. Delete the photo and update variables & UI |
| createNewPhotoButton | Adds a new camera+ button to right-most. Called after user takes a photo. |
| onClickTakePhoto | User clicked camera+ button. Start camera intent |
| dispatchTakePictureIntent | Start camera intent and save the taken photo into disk |
| onActivityResult | Framework override. When the photo is successfully taken, update variables & UI. Called after camera intent succeeds. |
| decodeSampledBitmapFromResource | This one resamples the taken photo, essentially shrinking it into less memory consuming bitmaps to be used in previews. |
| calculateInSampleSize | Used by decodeSampledBitmapFromResource. Calculates what dimensions the shrinked photo should have without harming the ratio. |
| onClickLoadPreview | User clicked one of the preview buttons. It is displayed on large preview |
| requestCameraPermission |  Only called on the very first start. Asks user if usage of camera is allowed |
| checkCameraPermission | Called each time app is started to check if the app has camera permissions. |
| MultiplePhotoTakerFragment | Factory pattern for fragment creation as recommended by Google. |
| onCreate | Framework override. Doesn't do much |

Two member variables are important

* This one maps the small preview buttons to their respective image files. 

```Java
HashMap<ImageButton, Uri>  button_uri_pair = new HashMap<>();
```

* This one is a horizontal layout inside a scroll view. Small preview buttons are added here.

```Java
LinearLayout layout_photo_buttons;
```


```xml
<HorizontalScrollView
  android:id="@+id/photo_buttons_scroll_view"
  android:background="@android:color/holo_blue_dark"
  android:minHeight="150dp"
  android:layout_width="match_parent"
  android:layout_height="match_parent">
  <LinearLayout
    android:id="@+id/layout_photo_buttons"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:orientation="horizontal" >
  </LinearLayout>
</HorizontalScrollView>
```

## Licence

Feel free to customise according to your needs.

[MIT](https://opensource.org/licenses/MIT)

# Source Code

[https://github.com/ayhanavci/Android-MultiPhotoTaker](https://github.com/ayhanavci/Android-MultiPhotoTaker)

```git pull https://github.com/ayhanavci/Android-MultiPhotoTaker.git```