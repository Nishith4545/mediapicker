# MediaPicker Library

This library is for apps requiring **broad access to photo and video files** located in shared storage. Apps using this library will need to pass an appropriate access review and demonstrate a core use case that requires **persistent or frequent** access to photo, image, or video files.

<div style="display: flex; justify-content: space-between;">
<img src="image_2024_11_28T10_43_23_770Z.png" alt="MediaPicker Screenshot" width="250"/>
<img src="image_2024_11_28T10_44_04_760Z.png" alt="MediaPicker Screenshot" width="250"/>
<img src="image_2024_11_28T10_45_10_636Z.png" alt="MediaPicker Screenshot" width="250"/>
<img src="image_2024_11_29T06_49_35_406Z.png" alt="MediaPicker Screenshot" width="250"/>
</div>

### Overview
The MediaPicker Library is designed for apps that require broad access to photo and video files. This library allows for persistent access to media files stored in shared storage, which is necessary for apps that need to access multiple images and videos on a frequent basis.

This library is ideal for apps that:

- Need to access multiple photos/videos regularly.
- Need to support core use cases that involve managing large media libraries.

### Features

- Provides access to media files stored in shared storage, allowing for broad and frequent access.
- Compliant with the latest Google policies but requires appropriate access review for approval.
- Supports both images and videos.
- Allows for media management features like bulk selection, organization, and access.

### Installation

To integrate the **Mediapicker** module into your Android project, follow these steps:

1. **Add the dependency** to your project’s `build.gradle` file (module level):

   ```gradle
   dependencies {
       //use latest version
       implementation 'com.github.Nishith4545:MediaPicker:x.x.x'
   }
   
The **Mediapicker Library** is a Kotlin-based Android library designed to simplify media selection in your apps. It automatically handles permissions related to media access and eliminates the need to manually declare them in your app's `AndroidManifest.xml`. Additionally, it supports **Android 14's Limited Access** feature, allowing users to grant access to specific photos and videos instead of all media files on their device.

## Features

### 1. **Automatic Permission Handling**
   - **Mediapicker Library** automatically manages runtime permissions for accessing photos and videos.
   - No need to manually declare permissions in your app's `AndroidManifest.xml`. The module handles everything internally.
   - Permissions are requested at runtime only when required, ensuring better security and compliance with Android's privacy policies.

### 2. **Support for Android 14's Limited Access Feature (Allow Limited Access)**
   - Fully supports **Android 14 (API level 34)** and its new **Limited Access to Photos and Videos** feature.
   - Users can select specific photos or videos to share with your app, providing better privacy controls.
   - The module ensures seamless handling of these restricted permissions on Android 14 and above, with no additional setup required from the developer.

 ### Usage
   - The Mediapicker Library provides an easy-to-use API to launch media selection and handle results, including permissions and access.

### In Your Fragment or Activity

To use the **Mediapicker Library**, you need to declare the `MediaSelectHelper` and configure it according to your media selection requirements.

#### Step 1: Initialize the Helper

```kotlin
lateinit var mediaSelectHelper: MediaSelectHelper
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mediaSelectHelper = MediaSelectHelper(this)
}
```

#### Step 2: Set Media Selection Options
For selecting images:
```kotlin
mediaSelectHelper.canSelectMultipleImages(false) // Set whether multiple images can be selected
mediaSelectHelper.selectOptionsForImagePicker(true) // Enable image picker options
```
For selecting videos:
```kotlin
mediaSelectHelper.canSelectMultipleVideo(false) // Set whether multiple videos can be selected
mediaSelectHelper.selectOptionsForVideoPicker() // Enable video picker options
```

#### Step 3: (Optional): Customize Limited Access Screen
To change the background color of the limited access media screen:
```kotlin
mediaSelectHelper.setLimitedAccessLayoutBackgroundColor(R.color.teal_200)
```

#### Step 4: Register Callbacks to Handle Selected Media
Use the callback function to retrieve selected media (image/video):
```kotlin
private fun setImagePicker() = with(binding) {
    mediaSelectHelper.registerCallback(object : MediaSelector {
        override fun onVideoUri(uri: Uri) {
            super.onVideoUri(uri)
            getPath(this@MainActivity, uri)?.let { it1 ->
                imageView.loadImagefromServerAny(it1)
            }
        }

        override fun onImageUri(uri: Uri) {
            uri.path?.let {
                imageView.loadImagefromServerAny(it)
            }
        }

        override fun onCameraVideoUri(uri: Uri) {
            uri.path?.let {
                imageView.loadImagefromServerAny(it)
            }
        }

        override fun onUpdatedStorageMedia(
            storageAccess: String,
            canSelectMultipleImages: Boolean,
            canSelectMultipleVideos: Boolean,
            selectFilter: String,
            mediaPath: String
        ) {
            super.onUpdatedStorageMedia(
                storageAccess,
                canSelectMultipleImages,
                canSelectMultipleVideos,
                selectFilter,
                mediaPath
            )
            imageView.loadImagefromServerAny(mediaPath)
        }
    }, supportFragmentManager)
}
```
