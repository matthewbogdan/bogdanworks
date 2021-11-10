---
title: "Adding Formspree Contact Form to Hugo website"
date: 2021-11-09T19:52:29-05:00
featured_image: "images/logo_Formspree.png"
description: "Adding a Formspree contact form to Hugo website."
categories: [website update]
tags: [form, form service, hugo, shortcode]
draft: false
---
[Formspree](https://formspree.io) is a form backend, API and email service for HTML & Javascript forms. It's a simple way to embed custom "contact us" forms on a [static website, like Hugo](https://gohugo.io). I'd like to add a [reCAPTCHA](https://developers.google.com/recaptcha/) to the form so I don't recieve too much spam mail. It's a simple process, but I'll go through the steps in how I set it up for this Hugo website. And you can see [my contact form](/contact) in action right on this site.

## Formspree Signup
The first step in using a [free form](https://formspree.io) is to sign up at https://formspree.io/register

If this is your first time signing up, you should immediately be taken to a _Form details for a new form_ page, but if not you can click _add new form_ on the https://formspree.io/forms page, provide a form name, project name and email and click create form.

{{< figure src="/images/20211109_Formspree_01-newform.png" link="/images/20211109_Formspree_01-newform.png" title="First Formspree form" alt="Formspree Form details for a new form" >}}

The formspree url can be posted right into your html form post attribute. This is listed as your Formspree endpoint where it directs you to you "Place this URL in the action attribute of your form."

this URL action can be added to your contact form page

{{< highlight markdown "linenos=table,hl_lines=15,linenostart=1" >}}
---
title: Contact
featured_image: "images/notebook.jpg"
omit_header_text: false
description: I'd love to hear from you
type: page
menu: main
---

I'd love to hear from you and your thoughts on IT, 
leadership, project management and life! Please fill out 
the form below and I'll get back to you as soon as I can! 
Thanks!

{{</* form-contact action="https://formspree.io/f/fayrmjpryb" */>}}

{{</ highlight >}}

At this point the form should work!

## But I would also like to add a reCAPTCHA with this form!

Go to https://www.google.com/recaptcha/admin and registering a new site.

{{< figure src="/images/20211109_Formspree_02-Register-Site-For-reCAPTCHA.png" link="/images/20211109_Formspree_02-Register-Site-For-reCAPTCHA.png" title="Registering a new site for reCAPTCHA" alt="Screenshot of Google Registering a new reCAPTCHA" >}}

Add a lable, recaptcha type ( chose reCAPTCHA v3, you can see more on [Introducing reCAPTCHA v3](https://www.youtube.com/watch?v=tbvxFW4UJdU)), domain, owner, accept the terms of service and click submit

Expand the reCAPTCHA keys and take note of them. You'll need the private key for formspree and the site key for your hugo shortcode.

{{< figure src="/images/20211109_Formspree_03-reCAPTCHA-Keys.png" link="/images/20211109_Formspree_03-reCAPTCHA-Keys.png" title="Attaining reCAPTCHA site and secret keys" alt="Screenshot of Google reCAPTCHA site and secret keys screen" >}}

## Adding reCAPTCHA secret key to Formsspree form

Go to the setting of your Formspree form, enable reCAPTCHA and past the secret key into the field:

{{< figure src="/images/20211109_Formspree_04-Add-reCAPTCHA-private-key.png" link="/images/20211109_Formspree_04-Add-reCAPTCHA-private-key.png" title="Adding reCAPTCHA secret key to Formspree form" alt="Screenshot of Adding reCAPTCHA secret key to Formspree form" >}}

## Adding reCAPTCHA site key to Hugo shortcode

Modifying the shortcode in the hugo website using the [gohugo-theme-ananke](https://themes.gohugio.io/themes/gohugo-theme-ananke/) by adding the recaptcha. You can see in line 17 "<div class="g-recaptcha" data-sitekey="YOURRECAPTCHASITEKEYGOESHERE"></div>," you need to replace the text above, "YOURRECAPTCHASITEKEYGOESHERE" with your key from the google reCAPTCHA admin screen.

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

## Testing the form:
Here is a screenshot of the [contact form on bogdanworks.com](/contact):

{{< figure src="/images/20211109_Formspree_05-Contact-Form.png" link="/images/20211109_Formspree_05-Contact-Form.png" title="Contact form on bogdanworks.com" alt="Screenshot of the Contact form on bogdanworks.com" >}}

Clicking submit sends you to the reCAPTCHA where the user has to confirm they are not a robot.

{{< figure src="/images/20211109_Formspree_06-reCAPTCHA-test.png" link="/images/20211109_Formspree_06-reCAPTCHA-test.png" title="Testing reCAPTCHA in form" alt="Screenshot of Testing reCAPTCHA in form" >}}

After selecting all images with traffic lights, we get a confirmation page!

{{< figure src="/images/20211109_Formspree_07-Form-Confirmation.png" link="/images/20211109_Formspree_07-Form-Confirmation.png" title="reCAPTCHA confirmation page" alt="Screenshot of reCAPTCHA confirmation page" >}}

And I received an email!

{{< figure src="/images/20211109_Formspree_08-Email-Received.png" link="/images/20211109_Formspree_08-Email-Received.png" title="Email sent to me" alt="Screenshot of email sent to me from contact form" >}}

And there you have it folks! Go ahead, [contact me!](/contactß)