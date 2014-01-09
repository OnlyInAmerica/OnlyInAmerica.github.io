---
layout: post
title:  "Android NDK & Gradle"
date:   2014-1-4 22:20:22
blurb:  "One small step toward a sane Android build environment."
categories: blog
---

I re-formatted my machine to kick off the new year and it's great. The clean slate makes you re-consider all sorts of decisions about your computing behavior and organization. One of my newest decisions is that I want to avoid installing Eclipse. I “admire” the flexibility of the platform which, through a rat’s nest of plugins, can accomplish more or less anything. It gives me a feeling of nostalgia for my high school Java class and its instructor, to whom I owe a ton. However as a former co-worker told me on my last day at the job “You’ve got to flush the toilet”.

The latest release of the Android Gradle plugin (0.7.3) seems to have quietly added support for the NDK: the one hold-out feature that prevents me from unequivocally recommending the switch from ANT, ADT and, of course, Eclipse. The NDK is the bridge to a universe of FOSS libraries and enables cross platform development.

So anyway, go check out the only real reference to Android’s Gradle NDK support in a [buried link to the Android gradle samples](http://tools.android.com/tech-docs/new-build-system/gradle-samples-0.7.3.zip). Look for the ndkSanAngeles sample. It looks like the new build system will replace the need to write Android Makefiles with some pretty groovy additions to your build.gradle. For example, to build a single example.c file in your project’s jni/ directory:

{% highlight groovy %}
//build.gradle
defaultConfig {
        ndk {
            moduleName "example"
			cFlags "-march=armv7-a -mfloat-abi=softfp -mfpu=neon"
			ldLibs "llog", "lz"
        }
    }
{% endhighlight %}

Effectively replaces the following Android Makefile:

{% highlight Make %}
#Android.mk
LOCAL_PATH := $(call my-dir)

APP_PLATFORM := android-XX
include $(CLEAR_VARS)

LOCAL_LDLIBS += -llog -lz
LOCAL_SRC_FILES := example.c
LOCAL_CFLAGS := -march=armv7-a -mfloat-abi=softfp -mfpu=neon
LOCAL_MODULE := example

include $(BUILD_SHARED_LIBRARY)


{% endhighlight %}

I like the direction [Xavier Durochet](https://plus.google.com/+XavierDucrohet/posts) and the Android tools gang are going. Hope they keep it up. 

Now a dream: Can we rethink [bionic](http://en.wikipedia.org/wiki/Bionic_(software\)) to include full glibc support and make native development really feel first-class?