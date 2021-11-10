---
title: "Formspree Contact Form"
date: 2021-11-09T19:52:29-05:00
featured_image: "images/logo_Formspree.png"
description: "Adding a contact form to a website using Formspree."
categories: [website update]
tags: [form, form service]
draft: false
---
Formspree is a form backend, API and email service for HTML & Javascript forms. It's a simple way to embed custom contact us forms on a static website. I'd like to add a reCAPTCHA to the form so I don't recieve too much spam mail. It's a simple process but I'll go through the steps in how I set it up for this site. And you can see [my contact form](/contact) in action right on this site.

## Formspree Signup
The first step in using a [free form](https://formspree.io) is to sign up at https://formspree.io/register


The formspree url can be posted right into your html form post attribute. This is listed as your Formspree endpoint where it directs you to you "Place this URL in the action attribute of your form."

But I would also like to add a reCAPTCHA with this form!

Going to https://www.google.com/recaptcha/admin and registering a new site








Modifying the shortcode in the hugo website using the [gohugo-theme-ananke](https://themes.gohugio.io/themes/gohugo-theme-ananke/) by adding the recaptcha
{{< highlight markdown "linenos=table,hl_lines=17,linenostart=1" >}}
{{ $.Scratch.Add "labelClasses" "f6 b db mb1 mt3 sans-serif mid-gray" }}
{{ $.Scratch.Add "inputClasses" "w-100 f5 pv3 ph3 bg-light-gray bn" }}
 
<form class="black-80 sans-serif" accept-charset="UTF-8" action="{{ .Get "action" }}" method="POST" role="form">
 
   <label class="{{ $.Scratch.Get "labelClasses" }}"  for="name">{{ i18n "yourName" }}</label>
   <input type="text" id="name" name="name" class="{{ $.Scratch.Get "inputClasses" }}"  required placeholder=" "  aria-labelledby="name"/>
 
   <label class="{{ $.Scratch.Get "labelClasses" }}" for="email">{{ i18n "emailAddress" }}</label>
   <input type="email" id="email" name="email" class="{{ $.Scratch.Get "inputClasses" }}"  required placeholder=" "  aria-labelledby="email"/>
   <div class="requirements f6 gray glow i ph3 overflow-hidden">
     {{ i18n "emailRequiredNote" }}
   </div>
 
   <label class="{{ $.Scratch.Get "labelClasses" }}" for="message">{{ i18n "message" }}</label>
   <textarea id="message" name="message" class="{{ $.Scratch.Get "inputClasses" }} h4" aria-labelledby="message"></textarea>
   <div class="g-recaptcha" data-sitekey="YOURRECAPTCHASITEKEYGOESHERE"></div>
   <input class="db w-100 mv2 white pa3 bn hover-shadow hover-bg-black bg-animate bg-black" type="submit" value="{{ i18n "send" }}" />
 
</form>
 
{{< / highlight >}}}

Replace the text above, "YOURRECAPTCHASITEKEYGOESHERE" with your key from the google reCAPTCHA admin screen.


## Testing the form:


Clicking sends you to the reCAPTCHA where the user has to confirm they are not a robot (I’m using reCAPTCHA 3.0)



After selecting all images with traffic lights, we get a confirmation page!


And I received an email!
