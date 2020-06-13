# CCB - A Free Cookie Consent Banner for Jekyll
CCB is a flexible and responsive cookie consent banner for Jekyll. It doesn't
require an additional plugin to be installed. It comes only with a set of 3 files.

## Prerequisites
- [Bootstrap 4](https://getbootstrap.com/)
- [jQuery 3](https://jquery.com/)
- [Font Awesome 6](https://fontawesome.com/)

A good introduction of how to setup jekyll with boostrap can you find here:

- [Creating a Jekyll Bootstrap Template](https://www.danielsieger.com/blog/2019/01/12/creating-jekyll-bootstrap-template.html)

## Features
- Responsiveness due to the use of bootstrap 
- Easily configurable via Jekyll's _config.yml
- Banner can be enabled on page level by using the frontmatter

## Installation
### Copy files
Copy the following 3 files to the corresponding directories of you Jekyll site.

```
+-- _includes
|   +-- cookieconsent.bootstrap.html
+-- assets
    +-- css
    |   +-- cookieconsent.bootstrap.css
    +-- js
        +-- cookieconsent.bootstrap.js
```

### Add CSS to your site
The CSS file has to be added to the header section where other CSS are referenced.

```html
<html>
  ...
  <link rel="stylesheet" href="/assets/css/cookieconsent.bootstrap.css">
</head>
```

### Include script code
Include cookieconsent.bootstrap.html right after the boostrap and jquery script tags. 
It contains the script tag and the initial function call.

```html
<script src="/assets/js/jquery-3.3.1.min.js"></script>
<script src="/assets/js/bootstrap.bundle.min.js"></script>
{% include cookieconsent.bootstrap.html %}
```

### Configuration
The last step is to add the configuration data to your _config.yml

```yaml
cookieconsent:
  style: "dark"
  notificationHead: "This web site uses cookies"
  notificationText:
    We use cookies for providing our services on this page. 
    While some of them are necessary, others help us to improve our online offering and operate it economically. 
    Accept the optional cookies or reject them by clicking on the \"Accept Only Selected\" button. 
    These settings can be revoked at any time. 
    For more information please refer to our <a href=\"/datapolicy\">data policy</a>.
    
  btnAcceptAll: "Accept All"
  btnAcceptSelected: "Accept Only Selected"
  btnMore: "More"
  cookies:
    -
      name: ccb_necessary
      title: "Essential"
      description: "Essential cookies enable basic functions and are necessary for the proper functioning of the website."
      expirydays: 365
      disabled: true
      allowed: true
    -
      name: ccb_analytics
      title: "Statistic"
      description: "Statistics cookies help to understand how visitors use websites by collecting and reporting information anonymously."
      expirydays: 365
      disabled: false
      allowed: false
    -
      name: ccb_advertising
      title: "Advertising"
      description: "Advertising cookies are used to display personalized advertising and to measure the effectiveness of advertising campaigns."
      expirydays: 365
      disabled: false
      allowed: false
```

## Usage
### Display the banner
The banner is displayed on each page load until the user has confirmed the cookie settings.
You can exclude pages by using the fronmatter section of that page.

```yaml
ccb_show_banner: false
```

The banner can be displayed by calling the showBanner() javascript function.

```html
<p><a href="javascript:ccb.showBanner();">Cookie Settings</a></p>
```
### Activate/Deactivate script code
Cookies are usually set by javascript code. (e.g. Google Analytics tracking code)
In order to avoid the script execution until the user has allowed the usage of the related script category, the script
execution has to be stopped. The necessary steps are:
- add or change the script type to text/plain
- add a class attribute to the script tag with the value "ccb-cookie-consent"
- add a ccb-cookie-type attribute to the script tag with the value which defines the script category

#### Google Analytics
The following example shows how the Google Analytics script has to be modified.

```html
<script src="https://www.googletagmanager.com/gtag/js?id=<your id>" type="text/plain" class="ccb-cookie-consent" ccb-cookie-type="ccb_analytics"></script>
<script type="text/plain" class="ccb-cookie-consent" ccb-cookie-type="ccb_analytics">
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', '<your id>', { 'anonymize_ip': true });
</script>
```

#### Google AdSense
For ad blocks inside your page the code looks like this:
```html
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="<some styles>"
     data-ad-client="<your pub id>"
     data-ad-slot="<slot id>"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>
```

You have to remove the script tag in front and after the ins-tag.

```html
<ins class="adsbygoogle"
     style="<some styles>"
     data-ad-client="<your pub id>"
     data-ad-slot="<slot id>"></ins>
```

Add the following lines before the closing body-tag.

```html
<script data-ad-client="<your pub id>" src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js" type="text/plain" class="ccb-cookie-consent" ccb-cookie-type="ccb_advertising"></script>
<script type="text/plain" class="ccb-cookie-consent" ccb-cookie-type="ccb_advertising">
  (adsbygoogle=window.adsbygoogle||[]).requestNonPersonalizedAds=ccb.isPersonalAdsAllowed() ? 0 : 1;
  $(".adsbygoogle").each(function () { (adsbygoogle = window.adsbygoogle || []).push({}); });
</script>
```

These changes will prevent loading of AdSense code at page load and waits until the cookies have been loaded
or the user has confirmed the usage of cookies.