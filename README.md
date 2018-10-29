# Xam.Plugins.AutoUpdate [In Development]

## Auto update for your Android/UWP

<div class="inline-block">
  <img src="https://github.com/angelinn/Xam.Plugin.UpdatePrompt/blob/master/images/update_android.png" alt="android" width="220"/>
</div>
<div class="inline-block" style="margin-left: 100px">
  <img src="https://github.com/angelinn/Xam.Plugin.UpdatePrompt/blob/master/images/update_uwp.png" alt="uwp" width="335"/>
</div>

TO DO: 
* Add docs how to poll from store
* Add option to open store instead of downloading app package

## What is it?
* Developer provides a check for updates function
* The plugin checks for updates every ```RunEvery``` period of time
* When a new version is available and the user clicks the **confirm** button, the file from the provided url is downloaded and started

## Works only with UWP and Android

## Installation
Nuget package will be available soon.

Install the package only on the Forms project.

## Android
For Android API > **23** a ```FileProvider``` configuration is required:
* Add to AndroidManifest
```xml
  <application android:label="...">
    <provider android:name="android.support.v4.content.FileProvider" android:authorities="com.companyname.application" android:grantUriPermissions="true" android:exported="false">
      <meta-data android:name="android.support.FILE_PROVIDER_PATHS" android:resource="@xml/file_paths" />
    </provider>
  </application>
```

* Create a new file - ```Resources/xml/file_paths.xml```
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
  <files-path name="files" path="/" />
</paths>
```

* Add to ```MainActivity```

```C#
AutoUpdate.Init(this, authority);

```

**NOTE:** The authority value is the same as the **android:authorities** in the ```AndroidManifest``` file.


## Usage

The ```UpdateManager``` class.

```C#
UpdateManager updateManager = new UpdateManager(
    title: "Update available",
    message: "A new version is available. Please update!",
    confirm: "Update",
    cancel: "Cancel",
    
    // choose how often to check when opening the app to avoid spamming the user every time
    runEvery: TimeSpan.FromDays(1), 
    checkForUpdatesFunction: async () =>
    {
        // check for updates from external url ...
        return new UpdatesCheckResponse
        {
            IsNewVersionAvailable = true,
            DownloadUrl = "http://site.com/file.apk"
        };
    }
);

```
