NEW! Checkout [this branch](https://github.com/iamnoah/writeCapture/tree/writeCapture2) for a completely rewritten writeCapture that should be much more compatible with a greater variety of scripts.

# Usage Summary #

    $(something).writeCapture().html('some html with document.write scripts');

or if you just want to force your ads to load async:

    $.writeCapture.autoAsync();

writeCapture can be used [with][plugin] or [without][nolib] jQuery and can be loaded asynchronously. For async loading, [[ControlJS]]  is a good option, but any async loading library or technique should work. See the [wiki](/iamnoah/writeCapture/wiki/Usage) for more detailed usage.

***Need help? See "Help!" below.***

# Dependencies #

writeCapture.js was developed using jQuery 1.3.2, but should work with most 
versions. We recommend you use jQuery, because it's pretty great, however, 
if that's not possible, there is support/nolib-support.js, which  provides 
a bare bones implementation of the support functions jQuery would otherwise 
provide and only adds about 800 bytes when Gzipped.

To use nolib-support, just grab the latest writeCapture-x.x.x.-nolib.min.js 
from the downloads section. It already includes writeCapture.js, so it's the 
only file you need. If you'd like the unminified source for debugging, simply 
include nolib-support.js before writeCapture.js.

Note that nolib does not implement `onLoad`, which is required for `autoAsync`.

If you already have another Ajax library you are using like Prototype or dojo,
you can [implement write capture support yourself](/iamnoah/writeCapture/wiki/WriteCaptureSupport).

#  Why should I care about writeCapture.js? #

Sometimes we are forced to use a third party script or markup that we simply 
cannot change (usually an Ad server). While our websites are sleek and snappy 
with a nearly pristine codebase and progressive enhancement and Ajax 
everywhere, many 3rd party libraries are still using tables for layout and 
the dreaded, *evil* `document.write` method.

What makes `document.write` so evil is that it is only "useful" for scripts inside
the body tag that are processed while the page is loading. If document.write is
called anytime after page load, it wipes out all the existing page content. 
That makes it very difficult to dynamically refresh content containing 
`document.write` calls.

Fortunately for you, difficult is not impossible, and writeCapture.js has 
already written (and extensively tested) the difficult part for you. All 
you have to do is give it the offending evil HTML and it will return nice
clean HTML, safe for injection into the document.

# Satisfied Customers

 * [TheDailyBeast.com](http://TheDailyBeast.com) [uses][jesus] writeCapture.js.

 * [8tracks.com](http://8tracks.com) "We were having horrible latency with our ad providers and your plugin saved the day! Very impressive work!"

If you use writeCapture and want some free advertising, let us know!
 
# Help!

First, please read the wiki, particularly the [Options](https://github.com/iamnoah/writeCapture/wiki/Options) page. There 
are a lot of workarounds to try before giving up.

Next, ask your question on the [mailing list](http://groups.google.com/group/writecapturejs-users). Please
ask your question [the smart way](http://catb.org/~esr/faqs/smart-questions.html#before). That includes creating
a stripped down and *working* example at http://jsfiddle.net/ so that everyone can play with it. A private page on your site 
demonstrating the problem is rarely helpful because we can't go and edit the source. Please take the time to 
reproduce it on JS fiddle so we can help you. 

Please **do not** email the author privately or open an issue in the issue tracker. The author does not actively
use write capture, so he probably does not have any idea what is causing your problem.

You may think that it is quicker to go straight to the mailing list or the author without taking the time to
isolate the problem and fully read and comprehend the documentation and  go over the source code, but anyone
who can help you has already done all of those things. Please respect our time, and you will get enthusiastic help.

# Caveats/Limitations #
 
* AdSense - AdSense code using `document.write` will not work. However AdSense provides
  [asynchronous tracking support][adsense], so you don't even need writeCapture to load 
  AdSense asynchronously.

* If any of the included scripts are on another domain, they will have to be 
  loaded asynchronously via script tag injection. Subsequent scripts in the 
  HTML will be blocked, so order will be preserved, but this means that not 
  all scripts may have yet run after the HTML is injected. This means that if 
  your script depends on all the scripts in the HTML having run, it should 
  utilize the done() callback.
  
* Scripts that assume that they are the last element in the document will 
  probably not function properly. This is rare, but if a script is uncouth 
  enough to use document.write, it's a possibility.  

# Control JS Support #

The built in `document.write` support in [Control JS](http://stevesouders.com/controljs/) 
is not great. It adds extra `span` tags and is not nearly as robust as a full library like writeCapture. That's why writeCapture provides a [plugin](/iamnoah/writeCapture/blob/master/cjs.writeCapture.js) to replace Control JS's `document.write` support with writeCapture.js.
  
For usage, see the [wiki](/iamnoah/writeCapture/wiki/ControlJS).

# extsrc Support #

[extsrc](http://code.google.com/p/extsrcjs/) support:

  // call from $.ready() or your document ready of choice
    writeCapture.extsrc(doneCallback);

You can use the same markup (`extsrc="..."`,`asyncsrc="..."`) but with a few
advantages:

  * All the power of writeCapture.js, so more `document.write` edge cases will work.
  * No extra span tags!
  * All scripts with `extsrc` will be run in order, so you don't have to worry about dependencies.

Note that you *do not* need extsrc.js, we are simply supporting the same syntax in writeCapture.


# Version History #

## 1.0.5 ##

  * Hacks:
  
    * Updated `proxyGetElementById` will attempt to copy attributes from the proxy 
    when jQuery is present. Should improve compatibility in some cases.
    
    * `writeOnGetElementById` for when proxying isn't enough.
    
  * New Experimental Feature: `autoAsync`. Inspired by 
     [eligrey](http://github.com/eligrey)'s 
     [async-document-write](http://github.com/eligrey/async-document-write).
  
  * All options and hacks can now be set globally and/or per call.
  
  * No more eval! Fixes a problem in IE where `var` variables were not visible 
  to other scripts.

## 1.0 ##

  * If it's good enough for [newsweek.com](http://newsweek.com) , it's good 
     enough to be version 1.0.

 * Fixed bug in pause/resume logic when using asyncAll that was preventing 
    execution from resuming.

 * Fixed error in IE when using `proxyGetElementById`.

## 0.9.5 ##

 * Big overhaul of the internal queuing system. It should be more robust and 
   100% deterministic from this point forward. Known to help with deeply nested
   cross domain scripts.

 * Fixed issue where scripts that include newlines in their src attribute were
   being truncated.
   
 * Added debugging support. See support/debug-support.js.

## 0.9.0 ##

 * writeCapture.js has matured enough to be close to a 1.0 release, after a
   little seasoning. If you have a problem with your Ad server or other code,
   submit a bug report!

 * Added hacks to support scripts that combine `document.write` with DOM 
   manipulation and iframes. Thanks to [jcugno](http://github.com/jcugno)
   for the bug reports and example code. Enabling `proxyGetElementById` 
   should help with some Ad servers.

## 0.3.3 ##

 * Fixed a number of issues with 'marginal' scripts (but if all scripts were in
   good shape, you wouldn't need writeCapture). Should help with some ad servers.
   Hat tip to [neskire](http://github.com/neskire) for raising the issues.

## 0.3.2 ##

 * Bugfix - jQuery 1.4.1 has a bug in replaceWith that mishandles strings. We 
   work around it by not using replaceWith.

## 0.3.1 ##
 
  * Added fixUrls hack to deal with encoded URLs.
 
  * Added MIT and GPL licenses, applied retroactively to all versions.

  * Optimized script loading (start loading in parallel immediately) if asyncAll is enabled.

## 0.3.0 ##
  
   * Replaced `sanitizeAll` with `sanitizeSerial`.

   * Created jQuery plugin.

   * Run all sanitized scripts through a global queue to prevent problems when
   multiple sanitizes with async loads are run in sequence.

## 0.2.1 ##
   
   * Added `html` and `replaceWith` convenience methods.

   * Added `load` convenience method.
 
## 0.2.0 ##
   
   * Only tasks that were running async (read: async script 
   loading with pause/resume) will result in a call to defer.
   
   * Allowed replacement of support functions by defining writeCaptureSupport
   so writeCapture can be used without jQuery.

   * Added nolib-support.js for using writeCapture without jQuery.

## 0.1 ##

   * first release. Any content but a single script tag would result in 
   partially async execution thanks to an overuse of defer.

[adsense]: https://code.google.com/apis/analytics/docs/tracking/asyncUsageGuide.html

[jesus]: http://iamnoah.blogspot.com/2009/12/github-taming-documentwrite-and-nodejs.html?showComment=1275069903219#c7207630670857732321 "It will save your site from hacks."

[nolib]: https://github.com/iamnoah/writeCapture/blob/master/support/nolib-support.js
[plugin]: https://github.com/iamnoah/writeCapture/tree/master/plugin
