[![Build Status](https://travis-ci.org/twofortyfouram/android-plugin-host-sdk-for-locale.png?branch=master)](https://travis-ci.org/twofortyfouram/android-plugin-host-sdk-for-locale)

# Overview
[Locale](https://play.google.com/store/apps/details?id=com.twofortyfouram.locale) allows developers to create plug-in conditions and settings through the [Locale Developer Platform](http://www.twofortyfouram.com/developer).  Interaction between Locale (host) and plug-ins (client) occurs via an Intent-based API.  This repo contains an SDK that implements the host functionality defined by the Intent-based API.  Although there are multiple ways to approach building a plug-in host, we recommend starting with this SDK layer.


# API Reference
JavaDocs for the library are published [here](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale).


# Compatibility
The SDK is compatible and optimized for Android API Level 8 and above.  Note, however, consuming the SDK requires at least Android Gradle Plugin 1.3.0. (This is due to a manifest placeholder in the AAR's manifest.)  For backwards compatibility, there are support-v4 versions of the Fragment and Loader classes in the SDK.


# Download
## Gradle
The library is published as an artifact to jCenter.  To use the library, the jCenter repository and the artifact need to be added to your build script.

The build.gradle repositories section would look something like the following:

    repositories {
        jcenter()
    }

And the dependencies section would look something like this:
    
    dependencies {
        compile group:'com.twofortyfouram', name:'android-plugin-host-sdk-for-locale', version:'[1.0.0,2.0['
    }


# Creating a Host
The Locale host SDK offers a simplified way to implement an application that can host plug-ins.  The host has three primary responsibilities with regards to plug-ins: 1. discovering plug-ins, 2. editing plug-in instances, and 3. executing plug-in instances.

A plug-in app is represented by the [Plugin](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/model/Plugin.html).  The data that the plug-in saved after editing is represented by [PluginInstanceData](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/model/PluginInstanceData.html).

## Discover
The [PluginRegistry](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/api/PluginRegistry.html) scans for installed plug-ins, which are represented by [Plugin](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/model/Plugin.html).  A host UI would most likely interact with the PluginRegistry through the [PluginRegistryLoader](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/ui/loader/PluginRegistryLoader.html) or [SupportPluginRegistryLoader](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/ui/loader/SupportPluginRegistryLoader.html).

## Edit
The host's UI displays to the user the Plugins that were discovered, using [Plugin.getActivityLabel()](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/model/Plugin.html#getActivityLabel-android.content.Context-) and [Plugin.getActivityIcon()](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/model/Plugin.html#getActivityIcon-android.content.Context-) to get human-readable information about the plug-in.

To edit a Plugin, the host instantiates a subclass of [AbstractPluginEditFragment](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/ui/fragment/AbstractPluginEditFragment.html) or [AbstractSupportPluginEditFragment](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/ui/fragment/AbstractSupportPluginEditFragment.html). AbstractPluginEditFragment takes care of launching the plug-in's "edit" Activity, processing the Activity result, serializing the plug-in's Bundle into a persistent form, and delivering a callback to subclasses via [handleSave(PluginInstanceData)](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/ui/fragment/AbstractPluginEditFragment.html#handleSave-com.twofortyfouram.locale.sdk.host.model.Plugin-com.twofortyfouram.locale.sdk.host.model.PluginInstanceData-).

## Execute
The host fires PluginInstanceData to a plug-in, via one of the two controller objects [Condition](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/api/Condition.html) or [Setting](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/api/Setting.html).

PluginInstanceData has getters for [getType()](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/model/PluginType.html) and [getRegistryName()](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/model/PluginInstanceData.html#getRegistryName--).  Those two getters provide the keys necessary to look up the associated Plugin from the PluginRegistry, enabling the host to instantiate [Condition](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/api/Condition.html) or [Setting](http://twofortyfouram.github.io/android-plugin-host-sdk-for-locale/com/twofortyfouram/locale/sdk/host/api/Setting.html).