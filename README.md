# Feature flags

Use feature flags to help test new features in production with a subset of users. 

## Requirements

* PHP 7.3+
* JavaScript support for all modern browsers (ES6)

## How it works

You can enable features by URL parameter.

The user passes the URL parameter `FEATURE_FLAG={featureName}` with multiple features separated by commas. For example to test a feature called "forest": http://example.com/my-page?FEATURE_FLAG=forest

When any feature is enabled, this sets a cookie named `FEATURE_FLAG` which stores which features are enabled. This helps identify subsequent requests as enabling this feature. By default this has a 4 hour lifetime. 

A JavaScript API is also provided so you can detect features in JS.

You can clear the current features by passing the URL parameter `FEATURE_FLAG_CLEAR` with any value, e.g. http://example.com/my-page?FEATURE_FLAG_CLEAR=1

## Checking the feature is enabled

You can check in your web browser if the feature is enabled for the current request, by checking your HTTP response headers and looking for `X-Strata-Feature` which will contain the list of features currently enabled. 

If you are using the JavaScript API the list of features is also outputted to the JavaScript console. 

## Caching

It is recommended when feature flags are enabled you mark the page cache as private (specific to one person).

```
Cache-Control: private
```

We are testing the use of Vary headers to enable full-page caching for groups of users. This is not considered stable at this time.

```
X-User-Feature: {featureName}
Vary: X-User-Feature
```

## Configuration

```php
use Strata\Feature\Feature;

$feature = new Feature();

// Add feature flag
$feature->addFeature('featureName');
```

## Usage

### PHP

```php
if ($feature->isEnabled('name')) {
	// do something
}
```

### Twig

```twig
{% if feature('name') %}
	<!-- do something -->
{ % endif %}
```

### JavaScript

```javascript
import {Feature} from "feature.js";

if (Feature.enabled("name")) {
	// do something
}
```

## Tracking features in analytics

TODO

## Data privacy & cookies

This software sets the following first-party cookie. You may want to add these details to your cookies page on your website if you are using Strata Feature in production.

* Name: STRATA_FEATURE
* Purpose: Used to track feature flags which help us improve the usability of our website
* Expires: 4 hours

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.

## Credits

This software is inspired by Etsy's [Feature API](https://github.com/etsy/feature).
