---
title: "Google Analytics Hugo Theme"
date: 2022-03-25T05:02:05-04:00
featured_image: "/images/logo_Hugo_GAnalytics.png"
description: "Updating Google Analytics code in Hugo site"
categories: ["SSG"]
tags: ["google analytics", "analytics", "hugo"]
draft: false
---

Google Analytics in my Hugo website
I initially was using the following code to track my google analytics however it wasn't working:
{{< highlight javascript "" >}}
<script type="application/javascript">
var doNotTrack = false;
if (!doNotTrack) {
	window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
	ga('create', 'G-3VXD2H9YHW', 'auto');
	
	ga('send', 'pageview');
}
</script>
<script async="" src="https://www.google-analytics.com/analytics.js"></script>

{{</highlight>}}

I'm using a Hugo theme which happens to be using a [Hugo internal template](https://gohugo.io/templates/internal/). I basically entered my tag manager measurement id into the yml config file and it constructed the javascript above.
But I was receiving the following message:

No data received from your website yet.
To start collecting data make sure your website is tagged using the Measurement ID.


Going to the tagging instruction on my Google Analytics account I found the following:
{{< highlight go "" >}}
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-3VXD2H9YHW"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-3VXD2H9YHW');
</script>

{{</highlight>}}

I'm going to remove the templated Google tag manager and create my own partial to add to the baseof.html file and see if it makes a difference. My partial looks like this:

{{< highlight go "" >}}
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id={{ .Site.Params.googleAnalytics
 }}"></script>
<script>
 window.dataLayer = window.dataLayer || [];
 function gtag(){dataLayer.push(arguments);}
 gtag('js', new Date());
 
 gtag('config', '{{ .Site.Params.googleAnalytics
 }}');
</script>

{{</highlight>}}

Note I'm using .Site.Params.googleAnalytics to reference a newly created params variable established in the config.yml file.

And changing the baseof.html from 
{{< highlight go "" >}}
{{ template "_internal/google_analytics_async.html" . }}
{{</highlight>}}

To
{{< highlight go "" >}}

{{ partial "google_analytics.html" . }}

{{</highlight>}}

To mock production I can run the hugo site using the following command:

{{< highlight bash "" >}}
HUGO_ENV=production hugo server
{{</highlight>}}

The javascript is now showing correctly in the page. I'm going to publish this update and check to make sure I'm seeing Google Analytics correctly.



