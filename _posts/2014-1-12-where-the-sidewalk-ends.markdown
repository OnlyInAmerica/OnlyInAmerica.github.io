---
layout: post
title:  "Free the Photosphere"
date:   2014-1-4 22:20:22
blurb:  "Fully spherical panorams are neat."
categories: blog
extra_js:
  - three.min.js
  - sphere.js
photosphere: PANO_20140112_152336.jpg
---
**SAFARI FUN-NOTE**: You have to [manually enable WebGL](https://discussions.apple.com/thread/3300585?start=0&tstart=0) in Safari or all hell breaks lose. Even so, it seems there are still [problems abound with three.js / WebGL](https://github.com/mrdoob/three.js/issues/3015).

Photospheres are a neat take on panoramic photos, except that they used to only display in Google products like Google+.

That was until [kennydude](https://github.com/kennydude/) created [sphere.js](https://github.com/kennydude/photosphere), a library for viewing Photospheres powered by [three.js](http://threejs.org/)! Oh and it's all FOSS. This is cool.

## What is a Photosphere?
A Photosphere is a spherical projection stored in a .jpeg along with some [extra EXIF data](https://developers.google.com/photo-sphere/metadata/?hl=en) in [XMP](http://en.wikipedia.org/wiki/Extensible_Metadata_Platform) format.

The same image rendered within an `<image>` tag:

![Flat Panorama](/img/PANO_FLAT_20140112_152336.jpg)

And an example of the extra EXIF data:
{% highlight xml %}
<rdf:Description rdf:about="" xmlns:GPano="http://ns.google.com/photos/1.0/panorama/">
    <GPano:UsePanoramaViewer>True</GPano:UsePanoramaViewer>
    <GPano:CaptureSoftware>Photo Sphere</GPano:CaptureSoftware>
    <GPano:StitchingSoftware>Photo Sphere</GPano:StitchingSoftware>
    <GPano:ProjectionType>equirectangular</GPano:ProjectionType>
    <GPano:PoseHeadingDegrees>350.0</GPano:PoseHeadingDegrees>
    <GPano:InitialViewHeadingDegrees>90.0</GPano:InitialViewHeadingDegrees>
    <GPano:InitialViewPitchDegrees>0.0</GPano:InitialViewPitchDegrees>
    <GPano:InitialViewRollDegrees>0.0</GPano:InitialViewRollDegrees>
    <GPano:InitialHorizontalFOVDegrees>75.0</GPano:InitialHorizontalFOVDegrees>
    <GPano:CroppedAreaLeftPixels>0</GPano:CroppedAreaLeftPixels>
    <GPano:CroppedAreaTopPixels>0</GPano:CroppedAreaTopPixels>
    <GPano:CroppedAreaImageWidthPixels>4000</GPano:CroppedAreaImageWidthPixels>
    <GPano:CroppedAreaImageHeightPixels>2000</GPano:CroppedAreaImageHeightPixels>
    <GPano:FullPanoWidthPixels>4000</GPano:FullPanoWidthPixels>
    <GPano:FullPanoHeightPixels>2000</GPano:FullPanoHeightPixels>
    <GPano:FirstPhotoDate>2012-11-07T21:03:13.465Z</GPano:FirstPhotoDate>
    <GPano:LastPhotoDate>2012-11-07T21:04:10.897Z</GPano:LastPhotoDate>
    <GPano:SourcePhotosCount>50</GPano:SourcePhotosCount>
    <GPano:ExposureLockUsed>False</GPano:ExposureLockUsed>
</rdf:Description>
{% endhighlight %}

## What Next?

Now that we understand the gist of what's going on, we can dream of creating gorgeous high-resolution panoramas with your DSLR that plop into your [Occulus Rift](https://bitbucket.org/wolfpld/panorift/overview).

Or maybe some custom hardware that instantly captures every constituent image with an array of cameras.

## Meta: How to include extra .js in a Jekyll post?

Using Jekyll's [Front Matter](http://jekyllrb.com/docs/frontmatter/), we can add custom variables to each post's Markdown file that are accessible in every template involved in rendering this post.

Here's this post's Front Matter:

{% highlight yaml %}
---
layout: post
title:  "Free the Photosphere"
date:   2014-1-4 22:20:22
blurb:  "Fully spherical panorams are neat."
categories: blog
extra_js:
  - three.min.js
  - sphere.js
photosphere: PANO_20140112_152336.jpg
---
{% endhighlight %}

And in my post template I add the following:

{% highlight html %}
{{ "{% if page.photosphere " }}%}
	<div id="photosphere" style="height:480px"></div>
	<script>
	window.onload = function() {
		new Photosphere("/img/{{ "{{page.photosphere" }}}}").loadPhotosphere(document.getElementById("photosphere"));
	};
	</script>
{{ "{% endif " }}%}
{% endhighlight %}

And at the bottom of the master layout:

{% highlight html %}
{{ "{% for js_name in page.extra_js " }}%}
    <script src="/js/{{ "{{js_name" }}}}"></script>
{{ "{% endfor " }}%}
{% endhighlight %}
## Meta Meta: How to highlight Liquid syntax in a Liquid template?

[Blow my mind](https://raw2.github.com/scottkf/tesoriere.com/master/_posts/2010-08-25-liquid-code-in-a-liquid-template-with-jekyll.markdown).



