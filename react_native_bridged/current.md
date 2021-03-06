<div class="callout callout--warning">
  <p class="callout__title">
    <span class="wysiwyg-font-size-large">Documentation Version</span>
  </p>
  <p>This documentation applies to version 20.11.0.</p>
  <p>
    To access the documentation for versions 20.04 and older, click
    <a href="/hc/en-us/articles/900003765726" target="_self" rel="undefined">here.</a>
  </p>
</div>
<div class="callout callout--info">
  <p class="callout__title">
    <strong><span class="wysiwyg-font-size-large">React Native Bridge is a successor to the React Native SDK</span></strong>
  </p>
  <p>
    The React Native SDK, which was developed earlier, will soon be end-of-life
    and will no longer be maintained.
    <span class="wysiwyg-color-black">Please use this bridge SDK for a more complete and tested version</span>.
  </p>
</div>
<p>
  <span style="font-weight:400">This is the Countly SDK for React Native applications. It features bridging, meaning it includes all the functionalities that Android and iOS SDKs provide rather than having those functionalities as React Native code.</span>
</p>
<h1>Creating a new application</h1>
<p>
  <span style="font-weight:400">In order to use the React Native SDK, please use the following commands to create a new React Native application.</span>
</p>
<pre><code class="shell">npm install -g react-native-cli     # Install React Native
react-native init AwesomeProject    # Create a new project

cd AwesomeProject                   # Go to that directory
react-native run-android # OR       # Run the android project
react-native run-ios                # Run the iOS project

# New terminal
adb reverse tcp:8081 tcp:8081       # Link Android port
npm start                           # Run the build server</code></pre>
<h1>Installing the Countly SDK</h1>
<p>
  <span style="font-weight:400">Run the following snippet in the root directory of your React Native project to install the npm dependencies and link&nbsp;</span><strong>native libraries</strong><span style="font-weight:400">.</span>
</p>
<pre># Include the Countly Class in the file that you want to use.<br>npm install --save https://github.com/Countly/countly-sdk-react-native-bridge.git<br># OR <br>npm install --save countly-sdk-react-native-bridge@20.11.0<br><br># Linking the library to your app<br><br># react-native &lt; 0.60. For both Android and iOS<br>react-native link countly-sdk-react-native-bridge<br>cd node_modules/countly-sdk-react-native-bridge/ios/<br>pod install <br>cd ../../../<br><br># react-native &gt;= 0.60 for iOS (Android will autolink)<br>cd ios <br>pod install<br>cd ..</pre>
<h1>Implementation</h1>
<p>
  <span style="font-weight:400">We will need to call two methods (<code>init</code> and <code>start</code>) in order to set up our SDK. You may also like to specify other parameters at this step (i.e. whether logging will be used). These methods should only be called once during the app's lifecycle and should be done as early as possible. Your main app component's <code>componentDidMount</code>method may be a good place.&nbsp;</span>
</p>
<p>
  <span style="font-weight:400">See the <code>serverURL</code> value in the code snippet below. </span>
</p>
<p>
  <span style="font-weight:400">If you are using the Countly Enterprise Edition trial servers, use the domain from which you are accessing your trial dashboard, such as <a href="https://try.count.ly" target="_self">https://try.count.ly</a></span><span style="font-weight:400">,</span><span style="font-weight:400">&nbsp;</span><a href="https://us-try.count.ly/"><span style="font-weight:400">https://us-try.count.ly</span></a>,<span style="font-weight:400">&nbsp;or&nbsp;</span><a href="https://asia-try.count.ly./"><span style="font-weight:400">https://asia-try.count.ly.</span></a><span style="font-weight:400"> </span>
</p>
<p>
  <span style="font-weight:400">If you use both Community Edition and Enterprise Edition, use your own domain name or IP address, such as </span><em><span style="font-weight:400">https://example.com</span></em><span style="font-weight:400">&nbsp;or&nbsp;</span><em><span style="font-weight:400">https://IP</span></em><span style="font-weight:400">&nbsp;(if SSL is setup).</span>
</p>
<pre><code class="javascript">import Countly from 'countly-sdk-react-native-bridge';

if(!await Countly.isInitialized()) {
Countly.setLoggingEnabled(true); // Enable countly internal debugging logs
Countly.enableCrashReporting(); // Enable crash reporting to report unhandled crashes to Countly
Countly.setRequiresConsent(true); // Set that consent should be required for features to work.
Countly.giveConsentInit(["location", "sessions", "attribution", "push", "events", "views", "crashes", "users", "push", "star-rating", "apm", "feedback", "remote-config"]); // give conset for specific features before init.
Countly.setLocationInit("TR", "Istanbul", "41.0082,28.9784", "10.2.33.12"); // Set user initial location.

await Countly.init("https://try.count.ly", "YOUR_APP_KEY"); // Initialize the countly SDK.
Countly.start(); // start session tracking
}</code></pre>
<p>
  <span class="wysiwyg-color-black" style="font-weight:400">After <code>init</code> and <code>start</code> have been called once, you may use the commands in the rest of this document to send additional data and metrics to your server.&nbsp;</span>
</p>
<h1>User Location</h1>
<p>
  <span>Countly allows you to send geolocation-based push notifications to your users. By default, the Countly Server uses the GeoIP database to deduce a user's location. However, if your app has a different way of detecting location, you may send this information to the Countly Server by using the <code>setLocationInit</code>&nbsp;or<code>setLocation</code> methods.</span>
</p>
<p>
  <span>We recommend using the <code>setLocationInit</code> method before initialization to sent location. This includes:</span>
</p>
<ul>
  <li>
    <code><span>countryCode</span></code><span>&nbsp;</span>a string in ISO 3166-1
    alpha-2 format country code
  </li>
  <li>
    <code>city</code><span>&nbsp;</span>a string specifying city name
  </li>
  <li>
    <code>location</code><span>&nbsp;</span>a string comma-separated latitude
    and longitude
  </li>
  <li>
    <code>IP</code><span>&nbsp;</span>a string specifying an IP address in IPv4
    or IPv6 formats
  </li>
</ul>
<pre><code class="javascript">// Example for setLocationInit
var countryCode = "us";
var city = "Houston";
var latitude = "29.634933";
var longitude = "-95.220255";
var ipAddress = "103.238.105.167";

Countly.setLocationInit(countryCode, city, latitude + "," + longitude, ipAddress);</code></pre>
<p>
  <span style="font-weight:400"><span>Geolocation recording methods may also be called at any time after the Countly SDK has started.<br>To do so, use the <code>setLocation</code> method as shown below.</span></span><span style="font-weight:400"></span>
</p>
<pre><code class="javascript">// Example for setLocation
var countryCode = "us";
var city = "Houston";
var latitude = "29.634933";
var longitude = "-95.220255";
var ipAddress = "103.238.105.167";

Countly.setLocation(countryCode, city, latitude + "," + longitude, ipAddress);
</code></pre>
<p>
  <span style="font-weight:400">To erase any cached location data from the device and stop further location tracking, use the following method. Note that i</span><span style="font-weight:400">f after disabling location, the <code>setLocation</code></span><span style="font-weight:400">is called with any non-null value, tracking will resume.</span>
</p>
<pre><code class="javascript">//disable location tracking
Countly.disableLocation();</code></pre>
<h1>Custom events</h1>
<p>
  <span style="font-weight:400">This is a quick summary on how to use custom events recording methods and what information they provide. For more information about how custom events and segmentations work, please review the </span><a href="http://resources.count.ly/docs/custom-events" target="_self" rel="undefined">Custom Events documentation</a><span style="font-weight:400">.</span>
</p>
<p>
  <span style="font-weight:400">Note that all data passed to the Countly instance via the SDK or API should be in UTF-8.</span>
</p>
<pre><code class="javascript">// Example for sending basic custom event
var event = {"eventName":"basic_event","eventCount":1};
Countly.sendEvent(event);

// Example for event with sum
var event = {"eventName":"Event With Sum","eventCount":1,"eventSum":"0.99"};
Countly.sendEvent(event);

// Example for event with segment
var event = {"eventName":"Event With Segment","eventCount":1};
event.segments = {"Country" : "Germany", "Age" : "28"};
Countly.sendEvent(event);

// Example for event with segment and sum
var event = {"eventName":"Event With Sum And Segment","eventCount":1,"eventSum":"0.99"};
event.segments = {"Country" : "Germany", "Age" : "28"};
Countly.sendEvent(event);

// Timed events
// Basic timed event
Countly.startEvent("timedEvent");
Countly.endEvent("timedEvent");

// Timed event with a sum
Countly.startEvent("timedEvent");
var event = {
  "eventName": "timedEvent",
  "eventSum": "0.99"
};
Countly.endEvent(event);

// Timed event with segment
Countly.startEvent("timedEvent");
var event = {
  "eventName": "timedEvent"
};
event.segments = {
  "Country": "Germany",
  "Age": "28"
};
Countly.endEvent(event);

// Timed event with segment, sum and count
Countly.startEvent("timedEvent");
var event = {
  "eventName": "timedEvent",
  "eventCount": 1,
  "eventSum": "0.99"
};
event.segments = {
  "Country": "Germany",
  "Age": "28"
};
Countly.endEvent(event);
</code></pre>
<h1>User Profiles</h1>
<p>
  <span style="font-weight:400">You can provide Countly any details you may have about your user or visitor. This will allow you to track each specific user on the "User Profiles" tab, which is available in Countly Enterprise Edition. For more details, please review the <a href="/hc/en-us/articles/360037630571" target="_self" rel="undefined">User Profiles documentation</a>.</span>
</p>
<p>
  <span style="font-weight:400">In order to set a user profile, use the code snippet below. After you send a user’s data, it can be found in your Dashboard under <code>Users &gt; User Profiles</code>.</span>
</p>
<pre><code class="javascript">// Example for setting user data
var options = {};
options.name = "Nicola Tesla";
options.username = "nicola";
options.email = "info@nicola.tesla";
options.organization = "Trust Electric Ltd";
options.phone = "+90 822 140 2546";
options.picture = "http://www.trust.electric/images/people/nicola.png";
options.picturePath = "";
options.gender = "M";
options.byear = 1919;
Countly.setUserData(options);</code></pre>
<p>
  <span style="font-weight:400">Countly also supports custom user properties that you can attach for each user. In order to set or modify a user's data (e.g. increment, multiply, etc.), use the code snippet below.</span>
</p>
<pre><code class="javascript">// examples for custom user properties
Countly.userData.setProperty("keyName", "keyValue"); //set custom property
Countly.userData.setOnce("keyName", 200); //set custom property only if property does not exist
Countly.userData.increment("keyName"); //increment value in key by one
Countly.userData.incrementBy("keyName", 10); //increment value in key by provided value
Countly.userData.multiply("keyName", 20); //multiply value in key by provided value
Countly.userData.saveMax("keyName", 100); //save max value between current and provided
Countly.userData.saveMin("keyName", 50); //save min value between current and provided
Countly.userData.setOnce("setOnce", 200);//insert value to array of unique values
Countly.userData.pushUniqueValue("type", "morning");//insert value to array of unique values
Countly.userData.pushValue("type", "morning");//insert value to array which can have duplicates
Countly.userData.pullValue("type", "morning");//remove value from array</code></pre>
<h1>Crash reporting</h1>
<p>
  <span style="font-weight:400">With this feature, the Countly SDK will generate a crash report if your application crashes due to an exception and send it to the Countly server for further inspection.</span>
</p>
<p>
  <span style="font-weight:400">If a crash report cannot be delivered to the server (e.g. no internet connection, unavailable server, etc.), then the SDK stores the crash report locally in order to try again at a later time.</span>
</p>
<p>
  <span style="font-weight:400">You will need to call the following method before calling <code>init</code> in order to activate automatic crash reporting.</span>
</p>
<pre><code class="javascript">// Using Countly crash reports
Countly.enableCrashReporting();<br>Countly.init(...);</code></pre>
<p>
  <span style="font-weight:400">You may also add breadcrumb crash logs to your crash report using <code class="javascript">addCrashLog</code></span><span style="font-weight:400"> and catch and send exceptions manually using <code class="javascript">logException</code></span><span style="font-weight:400">. Please review the example code below.</span>
</p>
<pre><code class="javascript">Countly.addCrashLog("User Performed Step A");
setTimeout(() =&gt; { Countly.addCrashLog("User Performed Step B");}, 1000);
setTimeout(() =&gt; { Countly.addCrashLog("User Performed Step C");}, 1500);
setTimeout(() =&gt; {
   try {
     var a = {};
     var x = a.b.c; // this will throw an error.
   } catch (exc) {   
     var stack = exc.stack.toString();
     const customSegments = {"external_lib_version": "4.2.7", "theme": "dark"};
     const nonfatal = true;
     Countly.logException(stack, nonfatal, customSegments);
   }
},2000);
</code></pre>
<p>
  <span style="font-weight:400">The method <code class="javascript">logException</code>takes a string for the stack trace, a boolean flag indicating if the crash is considered fatal or not, and a segments dictionary to add additional data to your crash report.</span>
</p>
<h1>View tracking</h1>
<p>
  <span style="font-weight:400">You may manually add your own views in your application, and each view will be visible under <code>Analytics &gt; Views</code></span><span style="font-weight:400">. Below you may see two examples of sending a view using the <code>Countly.recordview</code></span><span style="font-weight:400">&nbsp;function.</span>
</p>
<pre><code class="javascript">// record a view on your application
Countly.recordView("My Home Page");
Countly.recordView("Profile Page");</code></pre>
<p>
  <span style="font-weight:400"><span>While tracking views, you may want to add custom segmentation to them.</span></span>
</p>
<pre><code class="javascript">// record view with segments
Countly.recordView("View with Segment", {"version": "1.0", "_facebook_version": "0.0.1"})</code></pre>
<h1>Application Performance Monitoring</h1>
<p>
  The Performance Monitoring feature allows you to analyze your application's performance
  on various aspects.
  <span style="font-weight:400">For more details, please review the</span>
  <a href="https://support.count.ly/hc/en-us/articles/900000819806-Performance-monitoring" target="_self">Performance Monitoring documentation</a>.
</p>
<p>
  Here is how you can utilize the Performance Monitoring feature in your apps:
</p>
<p>
  First, you need to enable the Performance Monitoring feature:
</p>
<pre><code class="javascript">// Enable APM features.
Countly.enableApm();</code></pre>
<p>
  With this, the Countly SDK will start measuring some performance traces automatically,
  including app foreground time, app background time. Additionally, custom traces
  and network traces can be manually recorded.
</p>
<h2 id="app-start-time" class="anchor-heading">App Start Time</h2>
<p>
  For the app start time to be recorded, you need to call the<span>&nbsp;</span><code>appLoadingFinished</code><span>&nbsp;</span>method.
  Make sure this method is called after <code>init</code>.
</p>
<pre><code class="javascript">// Example of appLoadingFinished
await Countly.init("https://try.count.ly", "YOUR_APP_KEY");<br>Countly.appLoadingFinished();</code></pre>
<p>
  This calculates and records the app launch time for performance monitoring.<br>
  It should be called when the app is loaded and it&nbsp;successfully displayed
  its first user-facing view.&nbsp;E.g. <code>componentDidMount:</code>&nbsp;method
  or wherever is suitable for the app's flow. The time passed since the app has
  started to launch will be automatically calculated and recorded for performance
  monitoring. Note that the app launch time can be recorded only once per app launch.
  So, the second and following calls to this method will be ignored.
</p>
<h2>Custom trace</h2>
<p>
  <span><span class="Apple-converted-space">You may also measure any operation you want and record it using custom traces. First, you need to start a trace by using the <code class="objectivec">startTrace(traceKey)</code> method:</span></span>
</p>
<pre>Countly.<span>startTrace</span>(traceKey);</pre>
<p>
  Then you may end it using the
  <span><span class="Apple-converted-space"><code class="objectivec">endTrace(traceKey, customMetric)</code>method, optionally passing any metrics as key-value pairs:</span></span>
</p>
<pre>String traceKey = <span>"Trace Key"</span>;<br>Map&lt;String, int&gt; customMetric = {<br>  <span>"ABC"</span>: <span>1233</span>,<br>  <span>"C44C"</span>: <span>1337<br></span>};<br>Countly.<span>endTrace</span>(traceKey, customMetric);</pre>
<p>
  The duration of the custom trace will be automatically calculated upon ending.
</p>
<p>
  Trace names should be non-zero length valid strings. Trying to start a custom
  trace with the already started name will have no effect. Trying to end a custom
  trace with already ended (or not yet started) name will have no effect.
</p>
<p>
  You may also cancel any custom trace you started using the<span><span class="Apple-converted-space"> <code class="objectivec">cancelTrace(traceKey)</code>method:</span></span>
</p>
<pre>Countly.cancelTrace(traceKey);</pre>
<p>
  Additionally, if you need you may cancel <strong>all</strong> custom traces you
  started, using the
  <span><span class="Apple-converted-space"><code class="objectivec">clearAllTraces()</code>method:</span></span>
</p>
<pre>Countly.clearAllTraces(traceKey);</pre>
<h2>Network trace</h2>
<p>
  You may record manual network traces using the
  <code><span>recordNetworkTrace(networkTraceKey, responseCode, requestPayloadSize, responsePayloadSize, startTime, endTime)</span></code>&nbsp;method.
</p>
<p>
  A network trace is a collection of measured information about a network request.<br>
  When a network request is completed, a network trace can be recorded manually
  to be analyzed via Performance Monitoring later using the following parameters:&nbsp;
</p>
<p>
  - <code>networkTraceKey</code>: A non-zero length valid string<br>
  - <code>responseCode</code>: HTTP status code of the received response<br>
  - <code>requestPayloadSize</code>: Size of the request's payload in bytes<br>
  - <code>responsePayloadSize</code>: Size of the received response's payload in
  bytes<br>
  - <code>startTime</code>: UNIX timestamp in milliseconds for the starting time
  of the request<br>
  - <code>endTime</code>: UNIX timestamp in milliseconds for the ending time of
  the request
</p>
<pre>Countly.<span>recordNetworkTrace</span>(networkTraceKey, responseCode, requestPayloadSize, responsePayloadSize, startTime, endTime);</pre>
<h1>Remote Config</h1>
<p>
  <span style="font-weight:400"> Remote Config allows you to modify how your app functions or looks by requesting key-value pairs from your Countly server. The returned values may be modified based on the user profile. For more details, please review the</span><a href="https://resources.count.ly/docs/remote-config">&nbsp;<span style="font-weight:400">Remote Config documentation</span></a><span style="font-weight:400">. </span>
</p>
<pre><code class="javascript">// remoteConfigUpdate will update all remote config values
Countly.remoteConfigUpdate(function(data){ 
	console.log(data);
});

// updateRemoteConfigForKeysOnly will update only those values which keyname are pass in array
Countly.updateRemoteConfigForKeysOnly(["test1"],function(data){
	console.log(data);
});

// updateRemoteConfigExceptKeys will update only values except which keyname are pass in array
Countly.updateRemoteConfigExceptKeys(["test1"],function(data){
	console.log(data);
});
      
// getRemoteConfigValueForKey will fetch remote config values for keyname sent
Countly.getRemoteConfigValueForKey("test1",function(data){
	console.log(data);
});</code></pre>
<p>
  <span style="font-weight:400">To erase all the values downloaded from the server call the function below.</span>
</p>
<pre><code class="javascript">Countly.remoteConfigClearValues();
</code></pre>
<h1>User Consent Management</h1>
<p>
  <span style="font-weight:400">Being compliant with GDPR and other data privacy regulations, Countly provides ways to toggle different Countly tracking features on or off depending on a user's given consent. For more details, please review the </span><a href="https://resources.count.ly/docs/compliance-hub"><span style="font-weight:400">Compliance Hub plugin</span></a><span style="font-weight:400">&nbsp;documentation.</span><span style="font-weight:400"><br></span>
</p>
<p>
  <span>Currently, available features with consent control are as follows</span>:
</p>
<ul>
  <li>
    sessions - tracking when, how often, and how long users use your app
  </li>
  <li>events - allows sending custom events to the server</li>
  <li>views - allows tracking which views user visits</li>
  <li>location - allows sending location information</li>
  <li>crashes - allows tracking crashes, exceptions, and errors</li>
  <li>
    attribution - allows tracking from which campaign did the user come
  </li>
  <li>
    users - allows collecting and providing user information, including custom
    properties
  </li>
  <li>push - allows push notifications</li>
  <li>
    star-rating - allows sending the results from Rating Feedback widgets
  </li>
  <li>
    feedback - allows showing the results from Survey and NPS® Feedback widgets
  </li>
  <li>apm - allows application performance monitoring</li>
  <li>
    remoteConfig - allows downloading remote config values from your server
  </li>
</ul>
<p>
  <span style="font-weight:400">Since the React Native Bridge SDK employs our iOS and Android SDKs, you may also be interested in reviewing their relevant documentation on this topic (</span><a href="https://support.count.ly/hc/en-us/articles/360037753511-iOS-watchOS-tvOS-macOS#consents" target="_self" rel="undefined">iOS Consents</a><span style="font-weight:400">&nbsp;and&nbsp;</span><a href="https://support.count.ly/hc/en-us/articles/360037754031-Android-SDK#user-consent-management" target="_self" rel="undefined">Android Consents</a><span style="font-weight:400">).</span>
</p>
<p>
  <span style="font-weight:400">Next we will go over the methods that are available in this SDK.</span>
</p>
<table>
  <tbody>
    <tr>
      <th>Method</th>
      <th>Parameters / Examples</th>
      <th>Description</th>
    </tr>
    <tr>
      <td>
        <code>setRequiresConsent</code>
      </td>
      <td>boolean</td>
      <td>
        <p>
          <span style="font-weight:400">The requirement for checking consent is disabled by default. To enable it, you will have to call <code>setRequiresConsent</code></span><span style="font-weight:400">with <code>true</code></span><span style="font-weight:400">before initializing Countly. You may also pass a consent flag as true or false when you call <code>init</code></span><span style="font-weight:400">.</span>
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <code>giveConsentInit</code>
      </td>
      <td>string array of strings</td>
      <td>
        <p>
          <span style="font-weight:400">To add consent for a single feature (string parameter) or a subset of features (array of strings parameter). Use this method for giving consent before&nbsp;</span><span style="font-weight:400">initializing.<br></span>
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <code>giveConsent</code>
      </td>
      <td>string array of strings</td>
      <td>
        <p>
          <span style="font-weight:400">To add consent for a single feature (string parameter) or a subset of features (array of strings parameter). Use this method for giving consent after </span><span style="font-weight:400">initializing.</span>&nbsp;
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <code>removeConsent</code>
      </td>
      <td>string array of strings</td>
      <td>
        <p>
          <span style="font-weight:400">To remove consent for a single feature (string parameter) or a subset of features (array of strings parameter).</span>
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <code>giveAllConsent</code>
      </td>
      <td>none</td>
      <td>To give consent for all available features.</td>
    </tr>
    <tr>
      <td>
        <code>removeAllConsent</code>
      </td>
      <td>none</td>
      <td>To remove consent for all available features.</td>
    </tr>
  </tbody>
</table>
<pre><code class="javascript">// Usage examples

Countly.setRequiresConsent(true);

// for a single feature
Countly.giveConsentInit("events");
Countly.giveConsent("events");
Countly.removeConsent("events");

// for a subset of features
Countly.giveConsentInit(["events", "views", "star-rating", "crashes"]);
Countly.giveConsent(["events", "views", "star-rating", "crashes"]);
Countly.removeConsent(["events", "views", "star-rating", "crashes"]);

// for all available features
Countly.giveAllConsent();
Countly.removeAllConsent();</code></pre>
<p>
  <span style="font-weight:400">The string values corresponding to the features that will be used in the <code>giveConsent</code> or <code>removeConsent</code> </span><span style="font-weight:400">methods may be found&nbsp;</span><a href="https://support.count.ly/hc/en-us/articles/360037753291-SDK-development-guide#exposing-available-features-for-consent" target="_self"><span style="font-weight:400">here</span></a><span style="font-weight:400">. In addition, please review our platform SDK documents if the feature is applicable or not for that platform.</span>
</p>
<h1>Device ID</h1>
<p>
  <span style="font-weight:400"><span>When the SDK is initialized for the first time and no device ID is provided, a device ID will be generated by the SDK.</span></span><span style="font-weight:400"><span></span></span>
</p>
<p>
  <span style="font-weight:400"><span>For iOS: the device ID generated by the SDK is the Identifier For Vendor (IDFV).<br>For Android:&nbsp; the device ID generated by the SDK is the OpenUDID or Google Advertising ID.</span></span>
</p>
<p>
  <span style="font-weight:400"><span>You may provide your own custom device ID when</span> initializing the SDK using the method below.</span>
</p>
<pre>Countly.<span>init</span>(<span>SERVER_URL</span>, <span>APP_KEY, DEVICE_ID</span>)</pre>
<h2>
  Changing the Device ID<span style="font-weight:400"></span>
</h2>
<p>
  <span style="font-weight:400">You may configure or change the device ID anytime using the method below.<br></span>
</p>
<pre>Countly.<span>changeDeviceId</span>(DEVICE_ID, <span>ON_SERVER</span>);</pre>
<p>
  <span>You may either allow the device to be counted as a new device or merge existing data on the server. </span>If
  <code>onServer</code> is set to <code>true</code>,
  <span>the old device ID on the server will be replaced with the new one, and data associated with the old device ID will be merged automatically.<br>Otherwise, if&nbsp;<code>onServer</code> is&nbsp;set to <code>false</code>, the device will be counted as a new device on the server.<br></span>
</p>
<h2 id="temporary-device-id" class="anchor-heading">Temporary Device ID</h2>
<p>
  You may use a temporary device ID mode for keeping all requests on hold until
  the real device ID is set later.&nbsp;
</p>
<p>
  To enable this
  <span style="font-weight:400">when initializing the SDK, use the method below.</span>
</p>
<pre>Countly.<span>init</span>(<span>SERVER_URL</span>, <span>APP_KEY, "TemporaryDeviceID"</span>)</pre>
<p>
  To enable a temporary device ID <strong>after</strong> initialization, use the
  method below.
</p>
<pre>Countly.<span>changeDeviceId</span>(Countly."TemporaryDeviceID", <span>ON_SERVER</span>);</pre>
<p>
  <strong>Note:</strong> When passing the<span> </span><code>TemporaryDeviceID</code><span>&nbsp;</span>for
  the<span>&nbsp;</span><code>deviceID</code><span>&nbsp;</span>parameter, the
  argument for the <code>onServer</code>parameter does not matter.
</p>
<p>
  As long as the device ID value is<span> </span><code>TemporaryDeviceID</code>,
  the SDK will be in temporary device ID mode and all requests will be on hold,
  but they will be persistently stored.
</p>
<p>
  When in temporary device ID mode, method calls for presenting feedback widgets
  and updating remote config will be ignored.
</p>
<p>
  Later, when the real device ID is set using the<span> </span><code>Countly.<span>changeDeviceId</span>(DEVICE_ID, <span>ON_SERVER</span>);</code><span>&nbsp;</span>method,
  all requests which have been kept on hold until that point will start with the
  real device ID.
</p>
<h1>Enabling and disabling logging</h1>
<p>
  <span style="font-weight:400">If logging is enabled, our SDK will then print out debug messages regarding its internal state and problems encountered. Logging is disabled by default, but:</span>
</p>
<ul>
  <li>
    <span style="font-weight:400">Enabling it during development is advised. </span>
  </li>
  <li>
    <span style="font-weight:400">Enabling it before initialization is recommended so you may see the initialization process logs.</span>
  </li>
</ul>
<pre><code class="javascript">Countly.enableLogging();<br> Countly.init(...);
 ...<br> // to disable it again later 
 Countly.disableLogging();


</code></pre>
<h1>Forcing HTTP POST</h1>
<p>
  <span style="font-weight:400">If the data sent to the server is short enough, the SDK will use HTTP GET requests. In the event you would like an override so that HTTP POST may be used in all cases, call the <code>setHttpPostForced</code> function after you have called <code>init</code>. You may use the same function later in the app’s life cycle to disable the override. This function has to be called every time the app starts, using the method below.</span>
</p>
<pre><code class="javascript">  
// enabling the override
Countly.setHttpPostForced(true);
  
// disabling the override
Countly.setHttpPostForced(false);</code></pre>
<h1>Checking if init has been called</h1>
<p>
  <span style="font-weight:400">In the event you would like to check if init has been called, use the function below.</span>
</p>
<pre><code class="javascript">Countly.isInitialized().then(result =&gt; console.log(result)); // true or false
</code></pre>
<h1>Checking if onStart has been called</h1>
<p>
  <span style="font-weight:400">For some Android applications, there might be a use case where the developer would like to check if the Countly SDK onStart function has been called. To do so, use the call below.</span>
</p>
<pre><code class="javascript">Countly.hasBeenCalledOnStart().then(result =&gt; console.log(result)); // true or false </code></pre>
<h1>Push Notifications</h1>
<p>
  Please first check our
  <a href="/hc/en-us/articles/360037270012" target="_self" rel="undefined">Push Notifications documentation</a>
  to see how you can use this feature. Since Android and iOS handles notifications
  differently (from how you can send them externally to how they are handled in
  native code), we need to provide different instructions for these two platforms.
</p>
<h2>General Setup</h2>
<p>
  <span>First, when setting up Push for the React Native (Bridge) SDK, select the push token mode. This allows you to choose either test or production modes. Note that the </span>push
  token mode should be set before initialization. U<span style="font-weight:400">se the method below.</span>
</p>
<pre>// Important: call this method before init method<br>Countly.pushTokenType(Countly.messagingMode.DEVELOPMENT, "Channel Name", "Channel Description");<br>// Countly.messagingMode.DEVELOPMENT<br>// Countly.messagingMode.PRODUCTION<br>// Countly.messagingMode.ADHOC</pre>
<p>
  <span>When you are ready to initialize Countly Push, call <code>Countly.askForNotificationPermission()</code> after <code>init</code>, using the method below.</span>
</p>
<pre>// Call this method any time.<br>Countly.askForNotificationPermission();<br>// This method will ask for permission, <br>// and send push token to countly server.</pre>
<p>
  To register a Push Notification callback after initialising the SDK, use the
  method below.
</p>
<pre>Countly.registerForNotification(function(theNotification){<br>console.log(JSON.stringify(theNotification));<br>});</pre>
<h2>Android Setup</h2>
<p>
  Step 1:
  <span>For FCM credentials setup please follow the instruction from this URL</span><br>
  <a class="c-link" href="https://support.count.ly/hc/en-us/articles/360037754031-Android#getting-fcm-credentials" target="_blank" rel="noopener noreferrer" data-stringify-link="https://support.count.ly/hc/en-us/articles/360037754031-Android#getting-fcm-credentials" data-sk="tooltip_parent">https://support.count.ly/hc/en-us/articles/360037754031-Android#getting-fcm-credentials</a>.
</p>
<p>
  Step 2: Make sure you have <code>google-services.json</code> from
  <a href="https://firebase.google.com/">https://firebase.google.com/</a>
</p>
<p>
  Step 3: Make sure the app package name and the
  <code>google-services.json</code> <code>package_name</code> matches.
</p>
<p>
  Step 4: Place the <code>google-services.json</code> file inside
  <code>android/app</code>
</p>
<p>
  Step 5: Use google services latest version from this link
  <a href="https://developers.google.com/android/guides/google-services-plugin">https://developers.google.com/android/guides/google-services-plugin</a>
</p>
<p>
  Step 6: Add the following line in the file <code>android/build.gradle</code>
</p>
<pre><code class="JavaScript">buildscript {
    dependencies {
        classpath 'com.google.gms:google-services:4.3.2'
    }
}
</code></pre>
<p>
  Step 7: Add the following line in file <code>android/app/build.gradle</code>
</p>
<pre><code class="JavaScript">// Add this at the bottom of the file
apply plugin: 'com.google.gms.google-services'
</code></pre>
<h2>iOS Setup</h2>
<p>
  For iOS push notification please follow the instruction from this URL
  <a href="https://resources.count.ly/docs/countly-sdk-for-ios-and-os-x#section-push-notifications">https://resources.count.ly/docs/countly-sdk-for-ios-and-os-x#section-push-notifications</a>
</p>
<p>
  For React Native you can find <code>CountlyNotificationService.h/m</code> file
  under <code>Pods/Pods/CountlyPod/Core/CountlyNotificationService.h/m</code>
</p>
<p>
  You can drag and drop both .h and .m files from Pod to Compile Sources.
</p>
<div class="img-container">
  <img src="/hc/article_attachments/900006223363/Screenshot_2020-12-23_at_6.54.30_PM.png" alt="Screenshot_2020-12-23_at_6.54.30_PM.png">
</div>
<h1>Attribution analytics &amp; install campaigns&nbsp;</h1>
<p>
  <a href="https://count.ly/attribution-analytics">Countly Attribution Analytics</a>
  allows you to measure the performance of your marketing campaign by attributing
  installs from specific campaigns. This feature is available for the Enterprise
  Edition.
</p>
<p>Call the method below before initialization.</p>
<pre><span>// Enable to measure your marketing campaign performance by attributing installs from specific campaigns.</span><br>Countly.<span>enableAttribution</span>();</pre>
<p>
  <span>For iOS 14+ use the <code><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">Countly</span><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">.</span><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">recordAttributionID("IDFA")</span></code></span><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif"> function instead of <span><code>Countly.enableAttribution()</code></span></span>
</p>
<p>
  <span>You can use <code><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">Countly</span><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">.</span><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">recordAttributionID</span></code>&nbsp;function to specify IDFA for campaign attribution</span>
</p>
<pre><span><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">Countly</span><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">.</span><span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">recordAttributionID("</span>IDFA_VALUE_YOU_GET_FROM_THE_SYSTEM");</span><span></span></pre>
<p>
  <span>For iOS 14+ due to Apple changes regarding Application Tracking, you need to ask the user for permission to track the Application.</span><span></span>
</p>
<p>
  <span>For IDFA you can use this Plugin, which also supports iOS 14+ changes for Application tracking permission:<br></span><span><a href="https://github.com/ijunaid/react-native-advertising-id.git">https://github.com/ijunaid/react-native-advertising-id.git</a></span>
</p>
<p>
  <span>Here is how to use this plugin with the Countly attribution feature:</span>
</p>
<div>
  <pre>npm install --save <a href="https://github.com/ijunaid/react-native-advertising-id.git">https://github.com/ijunaid/react-native-advertising-id.git</a><br><br>cd ./ios<br>pod install<br><br><strong>NSUserTrackingUsageDescription</strong><br>Add "Privacy - Tracking Usage Description" in your ios info.plist file.<br><br><strong>#Example Code for Countly attribution feature to support iOS 14+.</strong><br><br>import RNAdvertisingId from 'react-native-advertising-id';<br><br>if (Platform.OS.match("ios")) {<br>RNAdvertisingId.getAdvertisingId()<br>.then(response =&gt; {<br>Countly.recordAttributionID(response.advertisingId);<br>})<br>.catch(error =&gt; console.error(error));<br>}<br>else {<br>Countly.enableAttribution(); // Enable to measure your marketing campaign performance by attributing installs from specific campaigns.<br>}</pre>
</div>
<h1>Receiving user feedback</h1>
<p>
  <span style="font-weight:400">There are a different ways of receiving feedback from your users: the Star-rating dialog, the Ratings widget, and the Surveys widgets (Surveys and NPS®).</span>
</p>
<p>
  <span style="font-weight:400">The Star-rating dialog allows users to give feedback as a rating from 1 to 5. The Ratings widget allows users to rate using the same 1 to 5 emoji-based rating system as well as leave a text comment. The Surveys widgets (Surveys and NPS®) allow for even more targeted feedback from users.</span>
</p>
<h2>Ratings widget</h2>
<p>
  <span style="font-weight:400">The rating widget displays a server-configured widget to your user devices.</span>
</p>
<div class="img-container">
  <img src="https://count.ly/images/guide/ea55d24-072bb00-t1.png">
</div>
<p>
  <span style="font-weight:400">All the text fields in the example above can be configured and replaced with a custom string of your choice.</span>
</p>
<p>
  <span style="font-weight:400">In addition to a 1 through 5 rating, it is possible for users to leave a text comment and an email, should the user like to be contacted by the app developer.</span>
</p>
<p>
  <span style="font-weight:400">Showing the rating widget in a single call involves a two-set process: b</span><span style="font-weight:400">efore it is shown, the SDK tries to contact the server to get more information about the dialog. Therefore, a network connection to it is needed.</span>
</p>
<p>
  <span style="font-weight:400">To display the widget after initializing the SDK, you will first need to get the widget ID from your server, as shown below.</span>
</p>
<div class="img-container">
  <img src="https://count.ly/images/guide/f773cf4-2dd58c6-t2.png">
</div>
<p>
  <span style="font-weight:400">Then, call the function to show the widget popup using the widget ID below.</span>
</p>
<pre><code class="javascript">Countly.showFeedbackPopup("WidgetId", "Button Text");</code></pre>
<h2>Star-rating dialog</h2>
<p>
  <span style="font-weight:400">The Star-rating integration provides a dialog for getting user feedback about an application. It contains a title, a simple message explaining its purpose, a 1 through 5-star meter for getting users rating, and a dismiss button in case the user does not want to give a rating.</span>
</p>
<p>
  <span style="font-weight:400">This star rating has nothing to do with Google Play Store ratings and reviews. It is simply for getting brief feedback from your users to be displayed on the Countly dashboard. If the user dismisses the star-rating dialog without giving a rating, the event will not be recorded.</span>
</p>
<pre><code class="javascript">Countly.showStarRating();</code></pre>
<p>
  The star-rating dialog's title, message, and dismiss button text may be customized
  either through the <code>init</code> function or the&nbsp;<code>SetStarRatingDialogTexts</code>
  function.&nbsp;
</p>
<pre><code class="javascript">Countly.SetStarRatingDialogTexts("Custom title", "Custom message", "Custom dismiss button text");</code></pre>
<h2>Surveys widget</h2>
<p>
  It is possible to display 2 kinds of Surveys widgets:
  <a href="https://support.count.ly/hc/en-us/articles/900003407386-NPS-Net-Promoter-Score-" target="_blank" rel="noopener">NPS</a>
  and
  <a href="https://support.count.ly/hc/en-us/articles/900004337763-Surveys" target="_blank" rel="noopener">Surveys</a>.
  <span class="wysiwyg-color-red">Both widgets are shown as webviews and t</span>hey
  both use the same code methods.&nbsp;
</p>
<p>
  Before any Surveys widget can be shown, you need to create them in your Countly
  Dashboard.
</p>
<p>
  When the widgets are created, you need to use 2 calls in your SDK: one to get
  all available widgets for a user and another to display a chosen widget.
</p>
<p>To get your available widget list, use the call below.</p>
<pre><code class="javascript">Countly.getAvailableFeedbackWidgets().then((retrivedWidgets) =&gt; {<br>},(err) =&gt; {<br>});</code></pre>
<p>
  From the callback, get the list of all available widgets that apply to the current
  device ID.
</p>
<p>The objects in the returned list look like this:</p>
<pre><code class="javascript">{ "WIDGET_TYPE" : "WIDGET_ID" }</code></pre>
<p>
  To determine what kind of widget that is, check the "type" value. The potential
  values are <code>survey</code> and <code>nps</code>.
</p>
<p>
  Then use the widget type and description (which is the same as provided in the
  Dashboard) to decide which widget to show.
</p>
<p>
  After you have decided which widget you want to display, call the function below.
</p>
<pre><code class="javascript">Countly.presentFeedbackWidget("WIDGET_TYPE", "WIDGET_ID", "CLOSE_BUTTON_TEXT");</code></pre>
<h1>Native C++ Crash Reporting</h1>
<p>
  <span style="font-weight:400">If you have C++ libraries in your Android app, the React Native Bridge SDK allows you to record possible crashes in your Countly server by integrating the <code>sdk-native</code></span><span style="font-weight:400">developed within our&nbsp;</span><a href="https://github.com/Countly/countly-sdk-android"><span style="font-weight:400">Android SDK</span></a><span style="font-weight:400">. F</span><span style="font-weight:400">ind more information <a href="https://support.count.ly/hc/en-us/articles/360037754031-Android-SDK#native-c-crash-reporting" target="_blank" rel="noopener">here</a>.</span>
</p>
<p>
  <span style="font-weight:400">As this feature is optional, you will need to uncomment some parts in our SDK files in order to make it available. </span>
</p>
<p>
  <span style="font-weight:400">1. Go to <code>android/build.gradle</code></span><span style="font-weight:400">and add the package dependency (all file paths in this section are given relative to the <code>countly-sdk-react-native-bridge</code></span><span style="font-weight:400">which was <code>npm</code></span><span style="font-weight:400">installed above):</span>
</p>
<pre><code class="shell">dependencies {
    implementation 'ly.count.android:sdk-native:19.02.3'    
}</code></pre>
<p>
  <span style="font-weight:400">2. Uncomment the following in the <code>android/src/main/java/ly/count/android/sdk/react/CountlyReactNative.java</code></span><span style="font-weight:400">file:</span>
</p>
<pre><code class="java">import ly.count.android.sdknative.CountlyNative;
    
@ReactMethod
  public void initNative(){
  CountlyNative.initNative(getReactApplicationContext());
}
	
@ReactMethod
  public void testCrash(){
  CountlyNative.crash();
}
    </code></pre>
<p>
  <span style="font-weight:400">3. Modify <code>Countly.js</code></span><span style="font-weight:400">to connect from JavaScript to these new methods:</span>
</p>
<pre><code class="javascript">Countly.initNative = function(){
    CountlyReactNative.initNative();
}

Countly.testCrash = function(){
    CountlyReactNative.testCrash();
}</code></pre>
<p>
  <span style="font-weight:400">If you are using our sample app in <code>example/App.js</code></span><span style="font-weight:400">,</span><span style="font-weight:400"> also uncomment the following parts in it for easy testing:</span>
</p>
<pre><code class="javascript">initNative(){
  Countly.initNative();
};

testCrash(){
  Countly.testCrash();
}

// ...

            &lt; Button onPress = { this.initNative } title = "Init Native" color = "#00b5ad"&gt; 
            &lt; Button onPress = { this.testCrash } title = "Test Native Crash" color = "crimson"&gt; 

</code></pre>
<p>
  <span style="font-weight:400">We suggest calling <code>Countly.initNative()</code></span><span style="font-weight:400"> as soon as your app is initialized to be able to catch setup time crashes. Sending crash dump files to the server will be taken care of by the Android SDK during the next app initialization. We also provide a Gradle plugin that automatically uploads symbol files to your server (these are needed for the symbolication of crash dumps). Integrate it into your React Native project as explained in the relevant Android documentation </span><a href="https://resources.count.ly/docs/countly-sdk-for-android#section-native-c-crash-reporting"><span style="font-weight:400">page</span></a><span style="font-weight:400">.</span>
</p>
<p>
  <span style="font-weight:400">This is what the debug logs will look like if you use this feature:</span>
</p>
<pre><code class="text">$ adb logcat -s Countly:V countly_breakpad_cpp:V

# when Countly.initNative() is called 

D/countly_breakpad_cpp(123): breakpad initialize succeeded. dump files will be saved at /Countly/CrashDumps

# when a crash occurs (you may trigger one by Countly.testCrash())

D/countly_breakpad_cpp(123): DumpCallback started
D/countly_breakpad_cpp(123): DumpCallback ==&gt; succeeded path=/Countly/CrashDumps/30f6d9b8-b3b2-1553-2efe0ba2-36588990.dmp

# when app is run again after the crash 

D/Countly (124): Initializing...
D/Countly (124): Checking for native crash dumps
D/Countly (124): Native crash folder exists, checking for dumps
D/Countly (124): Crash dump folder contains [1] files
D/Countly (124): Recording native crash dump: [30f6d9b8-b3b2-1553-2efe0ba2-36588990.dmp]</code></pre>
<h3>Pinned Certificate (Call this method before initialization)</h3>
<h4>Terminal</h4>
<pre>openssl s_client -connect try.count.ly:443 -showcerts</pre>
<p>
  Run the above command and copy the content inside the begin certificate and the
  end certificate.
</p>
<p>
  Example files:
  <a href="https://github.com/Countly/countly-sdk-react-native-bridge/blob/dev-nicolson/example/android.count.ly.cer" target="_blank" rel="noopener">Android</a>
  and
  <a href="https://github.com/Countly/countly-sdk-react-native-bridge/blob/dev-nicolson/example/ios.count.ly.cer" target="_blank" rel="noopener">iOS</a>.
  Note that Android needs the certificate string, while iOS needs the entire .cer
  file.
</p>
<h4>Android</h4>
<pre>cd AwesomeProject<br>ls<br>count.ly.cer<br>mkdir -p ./android/app/src/main/assets/<br>cp ./count.ly.cer ./android/app/src/main/assets/</pre>
<h4>iOS</h4>
<pre>open ./ios/AwesomeProject.xcworkspace<br>Right click on AwesomeProject and select `New Group` (ScreenShot 1)<br>Name it `Resources`<br>Drag and Drop count.ly.cer file under that folder (ScreenShot 2)<br>Make sure copy bundle resources has your certificate (Screenshot 4).</pre>
<pre><br><img src="/hc/article_attachments/900002229303/Screenshot_2020-07-07_at_11.39.02_AM.png" alt="Screenshot_2020-07-07_at_11.39.02_AM.png"><br><img src="/hc/article_attachments/900001515963/ScreenShot_Pinned_Certificate_1.png" alt="ScreenShot_Pinned_Certificate_1.png"></pre>
<p>
  <img src="/hc/article_attachments/900001515983/Screenshot_Pinned_Certificate_2.png" alt="Screenshot_Pinned_Certificate_2.png">
</p>
<pre><img src="/hc/article_attachments/900002229363/Screenshot_2020-07-07_at_11.39.40_AM.png" alt="Screenshot_2020-07-07_at_11.39.40_AM.png"></pre>
<h4>JavaScript</h4>
<pre>Countly.pinnedCertificates("count.ly.cer");</pre>
<p>
  Note that <code>count.ly.cer</code> is the name of the file. Replace this file
  with the one you have.
</p>
<h2>Interacting with the internal request queue</h2>
<p>
  When recording events or activities, the requests don't always get sent immediately.
  Events get grouped together. All the requests contain the same app key which
  is provided in the <code>init</code> function.
</p>
<p>
  There are two ways to interact with the app key in the request queue at the moment.&nbsp;
</p>
<p>
  1. You can replace all requests with a different app key with the current app
  key:&nbsp;
</p>
<pre>//Replaces all requests with a different app key with the current app key.<br>Countly.replaceAllAppKeysInQueueWithCurrentAppKey();</pre>
<p>
  In the request queue, if there are any requests whose app key is different than
  the current app key, these requests app key will be replaced with the current
  app key.<br>
  <br>
  <span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">2. You can remove all requests with a different app key in the request queue:</span>
</p>
<pre><span>//Removes all requests with a different app key in request queue.<br></span>Countly.removeDifferentAppKeysFromQueue();</pre>
<p>
  <span style="font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif">In the request queue, if there are any requests whose app key is different than the current app key, these requests will be removed from the request queue.</span>
</p>
<h2>Setting an event queue threshold</h2>
<p>
  Events get grouped together and are sent either every minute or after the unsent
  event count reaches a threshold. By default it is 10. If you would like to change
  this, call:
</p>
<pre>Countly.setEventSendThreshold(<span>6</span>);</pre>
<h2>&nbsp;</h2>