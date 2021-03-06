<p>
  This document explains how to download, setup, and use the Unity SDK for Countly.
  You can download the latest release from
  <a href="https://github.com/Countly/countly-sdk-unity/releases/" target="_self" rel="undefined">Github</a>.&nbsp;
</p>
<div class="callout callout--info">
  <p class="callout__title">
    <span class="wysiwyg-font-size-large"><strong>Older documentation</strong></span>
  </p>
  <p>
    To access the documentation for version 20.11.0 and older, click
    <a href="https://support.count.ly/hc/en-us/articles/360037813851-Unity" target="_self" rel="undefined">here.</a>
  </p>
</div>
<p>
  The following are some of the key assumptions being considered while developing
  the SDK. Please take into account the following <strong>before</strong> integrating
  this SDK:
</p>
<ol>
  <li>Scripting version is based on .NET 4.x equivalent</li>
  <li>API Compatibility Level is based on .NET 4.x</li>
</ol>
<h1>Integration</h1>
<p>
  Download the Unity package from
  <a href="https://github.com/Countly/countly-sdk-unity/releases" target="_blank" rel="noopener">GitHub</a>
  and import it into your project.
</p>
<div class="callout callout--info">
  <p class="callout__title">
    <strong><span class="wysiwyg-font-size-large">Adding Write Permission</span></strong>
  </p>
  <p>
    If you expect the game to be saved
    <span>on an SD card or any other type of external storage</span>, set
    <strong>Write Permission</strong><span>&nbsp;</span><span>to 'External (SDCard). This can be found in your Android platform settings under 'Other Settings'.</span>
  </p>
</div>
<p>
  For more information, check the sample app on
  <a href="http://github.com/countly/countly-sdk-unity" target="_blank" rel="noopener">Github</a>.&nbsp;
</p>
<h1>
  <span>Setting up the SDK</span>
</h1>
<p>
  Before you can use any functionality, you have to initiate the SDK.&nbsp;
</p>
<p>
  The shortest way to initiate the SDK is with this code snippet:
</p>
<pre>CountlyConfiguration config = <strong>new</strong> CountlyConfiguration<br>{<br>AppKey = <span>COUNTLY_APP_KEY,</span><br>ServerUrl = <span>COUNTLY_SERVER_URL</span>,<br>};<br><br>Countly.Instance.Init(config);</pre>
<p>
  To configure the SDK during init, a config object called "<strong>CountlyConfiguration</strong>"
  is used. The configuration is done by creating such an object. Afterward that
  config object is provided to the "Init" method.
</p>
<p>
  <span>To change the Configuration, update the values of parameters in the "<strong>CountlyConfiguration" </strong>object. Here are the details of the potential parameters:</span>
</p>
<p>
  <span><strong>appKey -&nbsp;</strong>(Mandatory, string) The “App Key” for the app that you created on the Countly server.&nbsp;<strong>Example:</strong>&nbsp;124qw3er5u678qwef88d6123456789qwertyui123.<br><strong>serverUrl -</strong>&nbsp;(Mandatory, string) The URL of the Countly server where you are going to post your requests.&nbsp;<strong>Example:</strong>&nbsp;<a href="https://us-try.count.ly/">https://try.count.ly/</a><br></span>
</p>
<p>
  <strong>deviceId -<span>&nbsp;</span></strong>(Optional, string) Your Device
  ID. It is an optional parameter.<span>&nbsp;</span><strong>Example:</strong><span>&nbsp;</span>f16e5af2-8a2a-4f37-965d-qwer5678ui98.
</p>
<p>Below you can find details of each parameter:</p>
<p>
  <strong>salt -<span>&nbsp;</span></strong>(Optional, string) Used to prevent
  parameter tampering. The default value is<span>&nbsp;</span><strong>NULL</strong>.&nbsp;
</p>
<p>
  <strong>enablePost -<span>&nbsp;</span></strong>(bool) When set to<span>&nbsp;</span><strong>true</strong>,
  all requests made to the Countly server will be done using HTTP POST. Otherwise,
  the SDK sends all requests using the HTTP GET method. In some cases, if the data
  to be sent exceeds the 1800-character limit, the SDK uses the POST method.<span>&nbsp;The default value is&nbsp;<strong>false</strong>.&nbsp;</span>
</p>
<p>
  <span><strong>enableTestMode -&nbsp;</strong>(Optional, bool) This parameter is useful in development when you don't want to send requests to the Countly server. The default value is<strong>&nbsp;false</strong>.</span>
</p>
<p>
  <strong>RequiresConsent -<span>&nbsp;</span></strong><span>(Optional, bool)&nbsp;</span>This
  is useful
  <span>during the app run when the user wants to opt-out of SDK features.</span>
</p>
<p>
  <strong>enableConsoleLogging -<span>&nbsp;</span></strong><span>(Optional, bool)&nbsp;</span>This
  parameter is useful when you are debugging your application. When set to<span>&nbsp;</span><strong>true</strong>,
  it basically turns on Logging.&nbsp;
</p>
<p>
  <strong>ignoreSessionCooldown -</strong><span>&nbsp;(Optional, bool)&nbsp;</span>Turns
  on/off the session cooldown behavior.<span>&nbsp;</span><span>The default value is&nbsp;<strong>false</strong>.&nbsp;</span>
</p>
<p>
  <strong>sessionDuration -</strong><span>&nbsp;(Optional, int)&nbsp;</span>Sets
  the interval (in seconds) after which the application will automatically extend
  the session, providing the manual session is disabled. This interval is also
  used to process requests in the queue. The default value is<strong><span>&nbsp;</span>60<span>&nbsp;</span></strong>(seconds).
</p>
<p>
  <strong>eventThreshold -</strong><span>&nbsp;(Optional, int) Sets&nbsp;</span>a
  threshold value that limits the number of events that can be recorded internally
  by the system before they can all be sent together in one request. Once the threshold
  limit is reached, the system groups all recorded events and sends them to the
  server. The default value is<span>&nbsp;</span><strong>100.</strong><span>&nbsp;</span>
</p>
<p>
  <strong>storedRequestLimit -</strong><span>&nbsp;(Optional, int)&nbsp;</span>Sets
  a threshold value that limits the number of requests that can be stored internally
  by the system. The system processes these requests after every sessionDuration
  interval has passed. The default value is<span>&nbsp;</span><strong>1000.</strong>
</p>
<p>
  <strong>notificationMode -<span>&nbsp;</span></strong>(Optional, enum) When<span>&nbsp;</span><strong>None</strong>,
  the SDK disables Push Notifications for the device. Use an<span>&nbsp;</span><strong>iOS Test Token<span>&nbsp;</span></strong>or
  an<span>&nbsp;</span><strong>Android Test Token</strong><span>&nbsp;</span>for
  testing purposes and in production use a<span>&nbsp;</span><strong>Production</strong><span>&nbsp;</span><strong>Token.</strong><span>&nbsp;</span>The
  SDK uses the supplied mode for sending Push Notifications. The default value
  is<span>&nbsp;</span><strong>None.</strong>
</p>
<p>
  <strong>enableAutomaticCrashReporting -<span>&nbsp;</span></strong><span>(Optional, bool) U</span>sed
  to turn on/off Automatic Crash Reporting. When set to<span>&nbsp;</span><strong>true</strong>,
  the SDK will catch exceptions and automatically report them to the Countly server.
  The default value is<span>&nbsp;</span><strong>true.</strong>
</p>
<h2 id="providing-the-application-key" class="anchor-heading">Providing the application key</h2>
<p>
  <span>Also called "AppKey" as shorthand. The application key is used to identify for which application this information is tracked. You receive this value by creating a new application in your Countly dashboard and accessing it in its application management screen.</span>
</p>
<p>
  <span><strong>Note:&nbsp;</strong>Ensure you are using the App Key (found under Management -&gt; Applications) and not the API Key. Entering the API Key will not work.</span>
</p>
<h2 id="providing-the-server-url" class="anchor-heading">Providing the server URL</h2>
<p>
  <span>If you are using Countly Enterprise Edition trial servers, use&nbsp;<code>https://try.count.ly</code>,&nbsp;<code>https://us-try.count.ly</code>&nbsp;or&nbsp;<code>https://asia-try.count.ly</code>&nbsp;It is basically the domain from which you are accessing your trial dashboard.</span>
</p>
<p>
  <span>If you use both Community Edition and Enterprise Edition, use your own domain name or IP address, such as&nbsp;</span><a href="https://example.com/"><span>https://example.com</span></a><span>&nbsp;or&nbsp;</span><a href="https://ip/"><span>https://IP</span></a><span>&nbsp;(if SSL has been set up).</span><span></span>
</p>
<h2 id="enabling-logging" class="anchor-heading">Enabling logging</h2>
<p>
  <span>The first thing you should do while integrating our SDK is enabling logging. If logging is enabled, then our SDK will print out debug messages about its internal state and encountered problems.&nbsp;</span>
</p>
<p>
  Set <code>EnableConsoleLogging</code> on the config object to enable logging:
</p>
<pre>CountlyConfiguration config = new CountlyConfiguration<br>{<br>AppKey = <span>COUNTLY_APP_KEY,</span><br>ServerUrl = <span>COUNTLY_SERVER_URL</span>,<br>EnableConsoleLogging = true<br>};</pre>
<h2 id="parameter-tampering-protection" class="anchor-heading">Parameter Tampering Protection</h2>
<p>
  <span>You may set the optional&nbsp;<code>salt</code></span><span>&nbsp;to be used for calculating the checksum of requested data which will be sent with each request, using the&nbsp;<code>&amp;checksum</code></span><span>&nbsp;field. You will need to set exactly the same&nbsp;<code>salt</code></span><span>&nbsp;on the Countly server. If&nbsp;the&nbsp;<code>salt</code></span><span>&nbsp;on the Countly server is set, all requests would be checked for the validity of the&nbsp;<code>&amp;checksum</code></span><span> field before being processed.</span>
</p>
<pre><code class="java hljs">configuration.Salt = "salt";</code></pre>
<h2 id="checking-if-init-has-been-called" class="anchor-heading">Accessing Countly Global Instance</h2>
<p>
  To access the Countly Global Instance use the following code snippet:
</p>
<pre>Countly.Instance.</pre>
<h2 id="checking-if-init-has-been-called" class="anchor-heading">Checking if init has been called</h2>
<p>
  <span>In case you would like to check if init has been called, you may use the following property:</span>
</p>
<pre><code class="java hljs">Countly.Instance.IsSDKInitialized;</code></pre>
<h1>Session handling</h1>
<p>The Unity SDK handles the session automatically.</p>
<p>
  <strong>Begin Session:</strong> the SDK is responsible for automatically handling
  the Countly session in your app. As soon as you call the initialization methods
  (Begin and SetDefaults) in your app start event, the SDK will start the session
  automatically (only when you set <code>enableManualSessionHandling</code> to
  true during initialization).
</p>
<p>
  <strong>Update Session:</strong> the SDK is responsible for automatically extending
  the session after every 60 seconds (default value). This value is configurable
  during initialization using the parameter sessionDuration. It cannot be modified
  after initialization.
</p>
<p>
  Note that in iOS, the session will not be extended when the app is in the background.
  As soon as the user switches back to the app, the session extension will resume.
  In Android, the session will extend on both occasions: foreground and background.
</p>
<p>
  <strong>End Session:</strong> the SDK is responsible for automatically ending
  the session whenever the user quits the application.
</p>
<p>
  The Session will be ended automatically when the user
  <span class="wysiwyg-color-black">calls </span><span style="background-color:#e9ebed;font-family:monospace, monospace;font-size:13px;white-space:pre">Application.Quit()</span>.
</p>
<h1>Custom Events</h1>
<h2>Setting up custom events</h2>
<p>
  <span style="font-weight:400">A&nbsp;</span><a href="http://resources.count.ly/docs/custom-events"><span style="font-weight:400">custom event</span></a><span style="font-weight:400"> is any type of action that you can send to a Countly instance, e.g. purchases, changed settings, view enabled, and so on, letting you get valuable information about your application.</span>
</p>
<p>
  The Unity SDK helps record as many events as you want (you can set a threshold
  limit during initialization), and the system will send them automatically to
  the server once the threshold limit is reached. By default, Countly tracks only
  up to 100 events. However, this is also configurable.
</p>
<h2>Accessing event-related functionalities</h2>
<p>
  In the SDK, all custom event-related functionalities can be browsed from the
  returned interface on:
</p>
<pre>countly.Events.</pre>
<p>Use the following code snippet to record an event:</p>
<pre>countly.Events.RecordEventAsync(string key, SegmentModel segmentation,<br>int? count = 1, double? sum = 0, double? duration = null);</pre>
<p>Here is the detail of the parameters:</p>
<ul>
  <li>
    <strong>key -&nbsp;</strong>(Mandatory, string) Event key
  </li>
  <li>
    <strong>segmentation -</strong> (Mandatory, SegmentModel) Custom segmentation
    you want to set, leave null if you don't want to add anything.
  </li>
  <li>
    <strong>count -</strong> (Optional, int) How many of these events have occurred,
    default value is "1".
  </li>
  <li>
    <strong>sum -</strong> (Optional, int) Set sum if needed, default value is
    "0".
  </li>
  <li>
    <strong>duration -</strong> set sum if needed, the default value is "0".
  </li>
</ul>
<h2>Segmentation</h2>
<p>
  When providing segmentation for events, the only valid data types are: "String",
  "Integer", "Double", and "Boolean". All other types will be ignored.
</p>
<h2>Custom event usage examples</h2>
<p>
  <span style="font-weight:400">Based on the example below of an event recording a <strong>purchase</strong>, h</span><span style="font-weight:400">ere is a quick summary of the information for each usage:</span>
</p>
<ul>
  <li>
    Usage 1: how many times the&nbsp;<strong>purchase</strong> event occurred.
  </li>
  <li>
    Usage 2: how many times the&nbsp;<strong>purchase</strong> event occurred
    + the total amount of those purchases.
  </li>
  <li>
    Usage 3: how many times the&nbsp;<strong>purchase</strong> event occurred
    +
    <span style="font-weight:400">from which countries and application versions those purchases were made.</span>
  </li>
  <li>
    Usage 4: how many times the&nbsp;<strong>purchase</strong> event occurred
    +&nbsp;<span style="font-weight:400">the total amount, both of which are also available, segmented into countries and application versions.</span>
  </li>
  <li>
    Usage 5: how many times the&nbsp;<strong>purchase</strong> event occurred
    +
    <span style="font-weight:400">the total amount, both of which are also available, segmented by countries and application versions + the total duration of those events.</span>
  </li>
</ul>
<h3>1. Event key and count</h3>
<pre><code class="java"><strong>await</strong> countly.Events.RecordEventAsync("purchase", count: 1);</code></pre>
<h3>2. Event key, count, and sum</h3>
<pre><code class="java"><strong>await</strong> countly.Events.RecordEventAsync(key: "purchase", count: 1, sum: 0.99);</code></pre>
<h3>3. Event key and count with segmentation(s)</h3>
<pre><code class="java">SegmentModel segmentation= new SegmentModel();

<strong>await</strong> countly.Events.RecordEventAsync(key: "<span>purchase</span>", segmentation: segment, count: 1);<br></code></pre>
<h3>4. Event key, count, and sum with segmentation(s)</h3>
<pre><code class="java">SegmentModel segmentation = new SegmentModel();
 segmentation.Add("country", "Germany");<br> segmentation.Add("app_version", "1.0");

<strong>await</strong> countly.Events.RecordEventAsync(key: "<span>purchase</span>", segmentation: segment, count: 1, sum: 0.99);</code></pre>
<h3>5. Event key, count, sum, and duration with segmentation(s)</h3>
<pre><code class="java">SegmentModel segmentation = new SegmentModel();
 segmentation.Add("country", "Germany");<br> segmentation.Add("app_version", "1.0");

<strong>await</strong> countly.Events.RecordEventAsync(key: "<span>purchase</span>", segmentation: segment, count: 1, sum: 0.99, duration: 60);<br></code></pre>
<p>
  <span style="font-weight:400">These are only a few examples of what you can do with Custom Events. You may go beyond those examples and use country, app_version, game_level, time_of_day, and any other segmentation of your choice that will provide you with valuable insights.</span>
</p>
<h1 id="crash-reporting" class="anchor-heading" tabindex="-1">Crash reporting</h1>
<p>
  <span>The Countly SDK for Unity can collect </span><a href="http://resources.count.ly/docs/introduction-to-crash-reporting-and-analytics"><span>Crash Reports</span></a><span>,</span><span>&nbsp;which you may examine and resolve later on the server.</span>
</p>
<h2>Automatic crash reporting</h2>
<p>
  The Unity SDK can automatically report uncaught exceptions/crashes in the application
  to the Countly server. To report uncaught exceptions/crashes automatically, enable
  <strong>enableAutomaticCrashReporting<span>&nbsp;</span></strong><span>in the SDK configuration.</span>
</p>
<h2 id="accessing-crashrelated-functionality" class="anchor-heading">Accessing crash-related functionalities</h2>
<p>
  In the SDK all crash-related functionalities can be browsed from the returned
  interface on:
</p>
<pre>countly.CrashReports.</pre>
<p>To log exception use the following code snippet:</p>
<pre><strong>await</strong> countly.CrashReports.SendCrashReportAsync(ex.Message, ex.StackTrace, LogType.Exception, null, false); </pre>
<p>Here is the detail of the parameters:</p>
<ul>
  <li>
    <strong>message -</strong> (<span>Mandatory</span>, string) a string that
    contains a detailed description of the exception.
  </li>
  <li>
    <strong>stackTrace -</strong> (<span>Mandatory, </span>string) A string that
    describes the contents of the call stack.
  </li>
  <li>
    <strong>type - </strong>(<span>Mandatory, </span>LogType) The type of the
    log message.
  </li>
  <li>
    <strong>segments - </strong>(Optional, ) Custom key/values to be reported.
  </li>
  <li>
    <strong>nonfatal -</strong>&nbsp; (Optional, bool) Set false if the error
    is fatal.
  </li>
</ul>
<h2 id="logging-handled-exceptions" class="anchor-heading">
  <span>Reporting nonfatal exception</span>
</h2>
<p>
  <span>You might catch an exception or similar error during your app’s runtime.</span>
</p>
<p>
  <span>You may also log these handled exceptions to monitor how and when they are happening.</span>
</p>
<p>Example:</p>
<pre><strong>try</strong><br> {<br><strong>    throw</strong> <strong>new</strong> DivideByZeroException();<br> }<br> <strong>catch</strong> (Exception ex)<br> {<br>    <strong>await</strong> countly.CrashReports.SendCrashReportAsync(ex.Message, ex.StackTrace, LogType.Exception); <br> }&nbsp;<br><br></pre>
<h2 id="logging-handled-exceptions" class="anchor-heading">
  <span>Reporting nonfatal exception with segmentation</span>
</h2>
<pre><span><br>Dictionary&lt;string, object&gt; segmentation = <strong>new</strong> Dictionary&lt;string, object&gt;{<br>{ "Action", "click"}<br>};<br><strong><br>try</strong><br>{<br><strong> throw</strong> <strong>new</strong> DivideByZeroException();<br>}<br><strong>catch</strong> (Exception ex)<br>{<br><strong>await</strong> countly.CrashReports.SendCrashReportAsync(ex.Message, ex.StackTrace, LogType.Exception, segmentation, true); <br>}&nbsp;</span></pre>
<h2 id="logging-handled-exceptions" class="anchor-heading">
  <span>Reporting fatal exception</span>
</h2>
<p>
  <span>If you have handled an exception and it turns out to be fatal to your app, you may use this call:</span>
</p>
<pre><strong>await</strong> countly.CrashReports.SendCrashReportAsync(ex.Message, ex.StackTrace, LogType.Exception, null, false); </pre>
<h2 id="logging-handled-exceptions" class="anchor-heading">
  <span>Reporting fatal exception with segmentation</span>
</h2>
<pre>Dictionary&lt;string, object&gt; segmentation = <strong>new</strong> Dictionary&lt;string, object&gt;{<br>{ "Action", "click"}<br>};<br><br><strong>await</strong> countly.CrashReports.SendCrashReportAsync(ex.Message, ex.StackTrace, LogType.Exception, segmentation, false); </pre>
<h2 id="adding-breadcrumbs" class="anchor-heading">Adding breadcrumbs</h2>
<p>
  Throughout your app, you can leave&nbsp;crash breadcrumbs
  <span>Mandatory that </span>which would describe previous steps that were taken
  in your app before the crash. After a crash happens, they will be sent together
  with the crash report.
</p>
<p>The following command adds a crash breadcrumb:</p>
<pre>countly.CrashReports.AddBreadcrumbs("breadcrumb");</pre>
<h1>View tracking</h1>
<p>
  The Countly Unity SDK supports manual view (screen) tracking. With this feature,
  you can report what views a user did and for how long. Thus, whenever there is
  a screen switch in your app, you can report it to the Countly server by using
  the following method:
</p>
<pre><strong>await</strong> countly.Views.RecordOpenViewAsync("Home Scene");</pre>
<p>
  <br>
  When the screen closes you can report it to the server by using the following
  method:
</p>
<pre><strong>await</strong> countly.Views.RecordCloseViewAsync("Home Scene");</pre>
<h1>View actions</h1>
<p>
  It is possible to report actions taken on views for heat maps or any other purpose.
  For that, use the following method:
</p>
<pre><strong>await</strong> countly.Views.ReportActionAsync(string type, int x, int y, int width, int height);</pre>
<p>All parameters are mandatory.</p>
<ul>
  <li>
    <strong>type -</strong> (string) Action type
  </li>
  <li>
    <strong>x -</strong> (int) Action's x-coordinate
  </li>
  <li>
    <strong>y -</strong> (int) Action's y-coordinate
  </li>
  <li>
    <strong>width&nbsp;-</strong> (int) Width of screen.
  </li>
  <li>
    <strong>height&nbsp;-</strong> (int) Height of screen.
  </li>
</ul>
<p>example:</p>
<pre><strong>await</strong> countly.Views.ReportActionAsync("Click", 300, 500, 720, 1280);</pre>
<h1>
  <span>User location</span>
</h1>
<p>
  <span>While integrating this SDK into your application, you might want to track your user location. You could use this information to better know your app’s user base. There are 4 fields that can be provided:</span>
</p>
<ul>
  <li>
    <span>Country code (two-letter ISO standard).</span>
  </li>
  <li>
    <span>City name (must be set together with the country code).</span>
  </li>
  <li>
    <span>Latitude and longitude values separated by a comma, e.g.</span><span>&nbsp;</span>"56.42345,123.45325".
  </li>
  <li>
    <span>Your user’s IP address.</span><span></span>
  </li>
</ul>
<p>
  <span>During init you can either disable location:<br></span>
</p>
<pre>config.DisableLocation();</pre>
<p>
  <span>or set location info that will be sent during the start of the user session:</span>
</p>
<pre>config.SetLocation(countryCode, city, gpsCoordinates, ipAddress);<br><br></pre>
<p>
  <span>Note that the ipAddress will only be updated if set through the init process.</span>
</p>
<pre><span>string countryCode = </span><span class="hljs-string">"us"</span><span>;<br>string city = </span><span class="hljs-string">"Houston"</span><span>;<br>string latitude = </span><span class="hljs-string">"29.634933"</span><span>; <br>string longitude = </span><span class="hljs-string">"-95.220255"</span><span>; <br>string ipAddress = </span><span class="hljs-keyword">null</span><span>;</span>&nbsp;<br><br>countly.Location.S<span>etLocation</span>(<span>countryCode, city, latitude + </span><span class="hljs-string">","</span><span> + longitude, ipAddress</span>);<br><br></pre>
<p>
  When those values are set, a separate request will be created to send them sent.
  Except for ip address, because Countly Server processes IP address only when
  starting a session.
</p>
<p>If you don't want to set specific fields, set them to null.</p>
<h2>Disabling location</h2>
<p>
  <span>Users might want to opt-out of location tracking. To do so, call:</span>
</p>
<pre>countly.Location.DisableLocation();</pre>
<p>
  <span>This action will erase the cached location data from the device and the server.</span>
</p>
<h1>
  <span class="wysiwyg-color-black">Ratings</span>
</h1>
<p>
  <span class="wysiwyg-color-black">Rating Is a customer satisfaction tool that collects direct user feedback and comments. For more details</span>,
  please see the
  <a href="/hc/en-us/articles/360037641291" target="_self">Ratings documentation</a>.
</p>
<p>
  <span>When a user rates your application, you can report it to the Countly server using the following method:</span><span></span>
</p>
<pre><span><strong>await</strong> countly.StarRatingReport.ReportStarRatingAsync(string platform, string appVersion, int rating);</span></pre>
<p>
  <span>All parameters are mandatory.</span>
</p>
<ul>
  <li>
    <span><strong>platform -</strong> (string) The name of the platform.</span>
  </li>
  <li>
    <span><strong>appVersion -</strong> (string) The current version of the app.</span>
  </li>
  <li>
    <span><strong>rating -</strong> (int) V</span><span>alue from 0 to 5 that will be set as the rating value.</span><span></span>
  </li>
</ul>
<p>
  <span>Example:</span>
</p>
<pre><strong>await</strong> countly.StarRatingReport.StarRatingAsync("android", "0.1", 3);</pre>
<h1 id="remote-config" class="anchor-heading" tabindex="-1">Remote Config</h1>
<p>
  <span>Available in the Enterprise Edition, Remote Config allows you to modify how your app functions or looks by requesting key-value pairs from your Countly server. The returned values may be modified based on the user profile. For more details, please see the </span><a href="https://resources.count.ly/docs/remote-config"><span>Remote Config documentation</span></a><span>.</span>
</p>
<h2 id="manual-remote-config-download" class="anchor-heading">Downloading Remote Config</h2>
<p>
  To download Remote Config, call <code>countly.RemoteConfigs.Update()</code>.
  After the successful download, the SDK stores the updated config locally.
</p>
<pre><strong>await</strong> countly.RemoteConfigs.Update();</pre>
<h2 id="getting-remote-config-values" class="anchor-heading">Getting Remote Config</h2>
<p>
  To access the stored config,&nbsp; call
  <code>countly.RemoteConfigs.Configs</code>. It will return <code>null</code>
  if there isn't any config stored.
</p>
<pre><strong>Dictionary</strong>&lt;<strong>string</strong>, <strong>object</strong>&gt; config = countly.RemoteConfigs.Configs;</pre>
<h1>User Profiles</h1>
<p>
  The Countly Unity SDK allows you to upload specific data related to a user to
  the Countly server. The SDK allows you to set the following predefined data for
  a particular user:
</p>
<ol>
  <li>Name: Full name of the user.</li>
  <li>Username: Username of the user.</li>
  <li>Email: Email address of the user.</li>
  <li>Organization: Organization the user is working in.</li>
  <li>Phone: Phone number.</li>
  <li>Picture Url: Web-based Url for the user’s profile.</li>
  <li>
    Gender: Gender of the user (use only single char like ‘M’ for Male and ‘F’
    for Female).
  </li>
  <li>Birth year: Birth year of the user.</li>
</ol>
<p>
  Apart from the above data, you can also add custom data for a user. The SDK allows
  you to upload user details using the methods listed below.
</p>
<h2 id="setting-up-user-profiles" class="anchor-heading" tabindex="-1">Setting up User Profiles</h2>
<p>
  <span>Available in the Enterprise Edition, User Profiles is a tool that helps you identify users, their devices, event timelines, and application crash information.&nbsp;</span>
</p>
<p>
  <span>You may send user-related information to Countly and let the Countly Dashboard show and segment this data. You may also send a notification to a group of users. For more information about User Profiles, review </span><a href="http://resources.count.ly/docs/user-profiles"><span>this documentation</span></a><span>.</span>
</p>
<p>Example:</p>
<pre><strong>var</strong> userDetails = <strong>new</strong> CountlyUserDetailsModel( "Full Name", "username", "useremail@email.com", "Organization", "222-222-222", "http://webresizer.com/images2/bird1_after.jpg", "M", "1986",<br>    <strong>new</strong> Dictionary&lt;string, object&gt; { <br>    { "Hair", "Black" }, <br>    { "Race", "Asian" }, <br>}); <br><strong>await</strong> countly.UserDetails.SetUserDetailsAsync(userDetails);</pre>
<h2>Setting up custom user details</h2>
<p>
  The SDK gives you the flexibility to send only the custom data to Countly servers,
  even when you don’t want to send other user-related data. Similar to the above
  method, it is also an instance method and not a static method. So, you first
  need to create an instance of the class <code>CountlyUserDetailsModel</code>.
  All the parameters expected in the constructor remain the same. You can leave
  all parameters as <strong>null</strong> and just provide the custom data segment
  for sending custom data to the Countly server.
</p>
<p>Example:</p>
<pre>var userDetails = <strong>new</strong> CountlyUserDetailsModel( <strong>new</strong> Dictionary&lt;string, object&gt; { <br>    { "Height", "5.8" }, <br>    { "Mole", "Lower Left Cheek" } <br>    });<br><strong>await</strong> countly.UserDetails.SetCustomUserDetailsAsync(userDetails);</pre>
<h2 id="modifying-custom-data" class="anchor-heading">Modifying custom data</h2>
<p>
  <span>You may also perform different manipulations to your custom data values, such as incrementing the current value on a server or storing an array of values under the same property.</span>
</p>
<p>
  <span>You will find the list of available manipulations below:</span>
</p>
<pre><code class="java hljs"><span class="hljs-comment">//set one custom properties</span>
<strong>await</strong> countly.UserDetails.Set(<span class="hljs-string">"test"</span>, <span class="hljs-string">"test"</span>);<br>
<span class="hljs-comment">//increment used value by 1</span>
<strong>await</strong> countly.UserDetails.Increment(<span class="hljs-string">"used"</span>);<br>
<span class="hljs-comment">//increment used value by provided value</span>
<strong>await</strong> countly.UserDetails.IncrementBy(<span class="hljs-string">"used"</span>, <span class="hljs-number">2</span>);<br>
<span class="hljs-comment">//multiply value by provided value</span>
<strong>await</strong> countly.UserDetails.Multiply(<span class="hljs-string">"used"</span>, <span class="hljs-number">3</span>);<br>
<span class="hljs-comment">//save maximal value</span>
<strong>await</strong> countly.UserDetails.Max(<span class="hljs-string">"highscore"</span>, <span class="hljs-number">300</span>);<br>
<span class="hljs-comment">//save minimal value</span>
<strong>await</strong> countly.UserDetails.Min(<span class="hljs-string">"best_time"</span>,<span class="hljs-number">60</span>);<br>
<span class="hljs-comment">//set value if it does not exist</span>
<strong>await</strong> countly.UserDetails.SetOnce(<span class="hljs-string">"tag"</span>, <span class="hljs-string">"test"</span>);<br>
<span class="hljs-comment">//insert value to array of unique values</span>
<strong>await</strong> countly.UserDetails.PushUnique(<span class="hljs-string">"type"</span>, new string[] {<span class="hljs-string">"morning"}</span>);<br>
<span class="hljs-comment">//insert value to array which can have duplicates</span>
<strong>await</strong> countly.UserDetails.Push(<span class="hljs-string">"type"</span>, new string[] {<span class="hljs-string">"morning"}</span>);<br>
<span class="hljs-comment">//remove value from array</span>
<strong>await</strong> countly.UserDetails.Pull(<span class="hljs-string">"type"</span>, new string[] {<span class="hljs-string">"morning"}</span>);

<span class="hljs-comment">//send provided values to server</span>
<strong>await</strong> countly.UserDetails.SaveAsync()<br></code></pre>
<p>
  In the end, always call
  <code><strong>await</strong> countly.UserDetails.SaveAsync();</code> to send
  them to the server.
</p>
<h2>Recording multiple update requests</h2>
<p>
  Apart from updating a single property in one request, you can modify multiple
  (unique) properties in one single request. This way you can increment Weight
  and multiply Height in the same request. Similarly, you can record any number
  of modified requests and Save them all together in one single request instead
  of multiple requests.
</p>
<p>
  Note that if you are going to modify multiple properties in one request, make
  sure your properties are unique, i.e. a property shouldn’t be modified more than
  once in a single request. However, if you record a property more than once, only
  the latest value will be posted to the server.&nbsp;
</p>
<p>Example:</p>
<pre>countly.UserDetails.Max("Weight", 90);<br>countly.UserDetails.SetOnce("Distance", "10KM");<br>countly.UserDetails.Push("Mole", new string[] { "Left Cheek", "Back", "Toe" }); ;<br><strong>await</strong> countly.UserDetails.SaveAsync();</pre>
<h1 id="user-consent-management" class="anchor-heading" tabindex="-1">User Consent management</h1>
<p>
  <span>In an effort to comply with GDPR, starting from 20.11.1, Unity Countly SDK provides ways to toggle different Countly features on/off depending on the given consent.</span>
</p>
<p>
  More information about GDPR can be found<span>&nbsp;</span><a href="https://blog.count.ly/countly-the-gdpr-how-worlds-leading-mobile-and-web-analytics-platform-can-help-organizations-5015042fab27">here</a>.
</p>
<p>
  <span>The requirement for consent is disabled by default. To enable it, you will have to set <code>RequiresConsent</code></span><span> value <code>true</code></span><span>&nbsp;before initializing Countly.</span>
</p>
<pre>CountlyConfiguration configuration = new CountlyConfiguration {<br>ServerUrl = "https://try.count.ly/",<br>AppKey = "YOUR_APP_KEY",<br>EnableConsoleLogging = true,<br>NotificationMode = TestMode.AndroidTestToken,<br>RequiresConsent = true<br>};<br><br>Countly.Instance.Init(configuration);</pre>
<p>
  <span>By default, no consent is given. That means that if no consent is enabled, Countly will not work and no network requests related to its features will be sent. When the consent status of a feature is changed, that change will be sent to the Countly server.</span>
</p>
<p>
  <span>For all features, except&nbsp;<code>push</code></span><span>, consent is not persistent and will have to be set each time before Countly init. Therefore, the storage and persistence of the given consent fall on the SDK integrator.</span>
</p>
<p>
  <span>Consent for features may be given and revoked at any time, but if it is given after Countly init, some features may only work in part.</span>
</p>
<p>
  <span>If consent is removed, but the appropriate function can't be called before the app closes, it should be done upon the next app start, so that any relevant server-side features may be disabled (such as the reverse geo IP for location).</span>
</p>
<p>
  <span>Feature names in the <strong>Unity SDK</strong> are stored as <strong>Enum</strong> called <code>Consents</code></span><span>.</span>
</p>
<p>The current features are:</p>
<p>
  *<span> </span><code>Sessions</code><span>&nbsp;</span>- tracking when, how often,
  and how long users use your app
</p>
<p>
  *<span> </span><code>Events</code><span>&nbsp;</span>- allow sending custom events
  to the server
</p>
<p>
  *<span> </span><code>Views</code><span>&nbsp;</span>- allow the tracking of which
  views user visits
</p>
<p>
  *<span> </span><code>Location</code><span>&nbsp;</span>- allow the sending of
  location information
</p>
<p>
  *<span> </span><code>Crashes</code><span>&nbsp;</span>- allow the tracking of
  crashes, exceptions, and errors
</p>
<p>
  *<span> </span><code>Users</code><span>&nbsp;</span>- allow the collecting/providing
  of user information, including custom properties
</p>
<p>
  *<span> </span><code>Push</code><span>&nbsp;</span>- allow push notifications
</p>
<p>
  *<span> </span><code>StarRating</code><span>&nbsp;</span>- allow their rating
  and feedback to be sent
</p>
<p>
  *<span> </span><code>RemoteConfig</code><span>&nbsp;</span>- allow downloading
  remote config values from your server
</p>
<h2>Give Specific Consents&nbsp;</h2>
<p>
  In case consent is required, you may give consent to features before the SDK
  Init call. These features consents are not persistent and must be given on every
  restart.
</p>
<pre><code class="java hljs"><span class="hljs-comment">// prepare consents that should be given</span></code><br><code class="java hljs">Consents[] consents = new Consents[] { Consents.Users, Consents.Location;</code><br><code class="java hljs"><span class="hljs-comment">// give consents to the features</span></code><br><code class="java hljs">configuration.GiveConsent(consents);</code></pre>
<h2 id="feature-groups" class="anchor-heading">
  <span>Consents</span> groups
</h2>
<p>
  <span>Consents may be put into groups. By doing this, you may give/remove consent to multiple features in the same call. They may be created using <code>CreateConsentGroup</code></span><span>. Those groups are not persistent and must be created on every restart. Consents to groups may be given to using <code class="java hljs">GiveConsentToGroup</code>.</span>
</p>
<pre><code class="java hljs"><span class="hljs-comment">// prepare consents that should be added to the group</span></code><br><code class="java hljs">Consents[] <span class="hljs-comment">consents</span> = new Consents[] { Consents.Users, Consents.Location;</code><br><code class="java hljs"><span class="hljs-comment">// create the Consent group</span></code><br><code class="java hljs">configuration.CreateConsentGroup("User-Consents", <span class="hljs-comment">consents</span>);</code><br><code class="java hljs"><span class="hljs-comment">// give consent to the provide consent group</span></code><br><code class="java hljs">configuration.GiveConsentToGroup("User-Consents");</code></pre>
<h2 id="changing-consent" class="anchor-heading">Changing consent</h2>
<p>
  <span>There are 3 ways of changing feature consent: </span>
</p>
<ul>
  <li>
    <span>&nbsp;<code>GiveConsent</code>/<code>RemoveConsent</code></span><span>&nbsp;- gives or removes consent to a specific feature.</span>
  </li>
</ul>
<pre><span><span class="hljs-comment"><code class="java hljs">// give consent to "sessions" feature</code></span> <br><span class="hljs-comment"><code class="java hljs">Countly.Instance.Consents.GiveConsent(new Consents[] { Consents.Sessions});</code></span><br><span class="hljs-comment"><code class="java hljs">// remove consent from "sessions" feature </code><br></span><span class="hljs-comment"><code class="java hljs">Countly.Instance.Consents.RemoveConsent(new Consents[] { Consents.Sessions});</code></span> <br></span></pre>
<ul>
  <li>
    <code>GiveConsentToGroup</code> / <code>RemoveConsentOfGroup</code>-
    <span>gives or removes</span> consent for a feature group.
  </li>
</ul>
<pre><code>// prepare array of groups</code><br>string[] groupName = new string[]{ "User-Consents", "Events-Consents" };<br><br><span><span class="hljs-comment"><code class="java hljs">// give consent to groups</code></span></span><br>Countly.Instance.Consent.GiveConsentToGroup(groupName);<br><br><span><span class="hljs-comment"><code class="java hljs">// remove consent of groups </code></span></span><br>Countly.Instance.Consent.RemoveConsentOfGroup(groupName);</pre>
<ul>
  <li>
    <code>GiveConsentAll</code> / <code>RemoveAllConsent</code>-
    <span>gives or removes</span> all consents.
  </li>
</ul>
<pre><code>// Give consent to all features</code><br>Countly.Instance.Consent.GiveConsentAll();<br><br><span><span class="hljs-comment"><code class="java hljs">// Remove consent from all features</code></span></span><br>Countly.Instance.Consent.RemoveAllConsent();</pre>
<h1>Changing Device ID</h1>
<p>
  The Countly Unity SDK persists Device ID when you provide it during initialization
  or generates a random ID when you don’t provide it. This Device ID will be used
  persistently for all future requests made from a device until you change that.
</p>
<p>
  The SDK allows you to change the Device ID at any point in time. You can use
  any of the following two methods to changing the Device ID, depending on your
  needs.
</p>
<h2>
  <span>Changing Device ID without server merge</span>
</h2>
<p>
  This method changes the Device ID and does the following other operations:
</p>
<ol>
  <li>Ends all the events that have been recorded until now.</li>
  <li>Ends the current session.</li>
  <li>
    Updates Device ID and starts a new session with a new Device ID.
  </li>
</ol>
<p>Example:</p>
<pre><strong>await</strong> countly.Device.ChangeDeviceIDAndEndCurrentSessionAsync("New Device Id");</pre>
<h2>
  <span>Changing Device ID with server merge</span>
</h2>
<p>
  This method changes the Device ID and merges data for both Device IDs in the
  Countly server.
</p>
<p>Example:</p>
<pre><strong>await</strong> countly.Device.ChangeDeviceIDAndEndCurrentSessionAsync("New Device Id");</pre>
<h1>Push Notification</h1>
<p>
  The Countly Unity SDK supports
  <span>FCM (Firebase Cloud Messaging) for Android. By default Push Notifications are disabled. To enable them to set Notification Mode in the Configuration.<br><br></span>
</p>
<pre><span>CountlyConfiguration config = new CountlyConfiguration<br>{<br>AppKey = COUNTLY_APP_KEY,<br>ServerUrl = COUNTLY_SERVER_URL,<br>EnableConsoleLogging = true,<br>NotificationMode = TestMode.AndroidTestToken<br>};</span></pre>
<p>&nbsp;</p>
<p>
  <span>Use an <strong>iOS Test Token </strong>or an <strong>Android Test Token</strong> for testing purposes and in production use a <strong>Production</strong> <strong>Token</strong></span>
</p>
<h2>
  <span>Android&nbsp;</span>
</h2>
<h3 id="getting-fcm-credentials" class="anchor-heading">FCM credentials</h3>
<p>
  <span>The Countly server needs an FCM server key to send notifications through FCM. </span><span></span>
</p>
<p id="integrating-fcm-into-your-app" class="anchor-heading">
  To set it up, refer to
  <a href="https://support.count.ly/hc/en-us/articles/360037754031-Android-SDK#getting-fcm-credentials" target="_self" rel="undefined">Android documentation</a>.
</p>
<h3 class="anchor-heading">Integrating FCM into your app</h3>
<ol>
  <li>
    <span>Download google-services.json from </span><a href="https://console.firebase.google.com/">Firebase console.</a>
  </li>
  <li>
    <span>Create google-services.xml from google-services.json. You can use an online converter </span><a href="https://dandar3.github.io/android/google-services-json-to-xml.html" rel="nofollow">here</a><span>.</span>
  </li>
  <li>
    <span>Put your file google-services.xml in /Plugins/Android/Notifications/res/values (replace if necessary).</span>
  </li>
</ol>
<h3>
  <span style="font-size:1.2em;font-weight:600">Changing </span><span style="font-size:1.2em;font-weight:600">N</span><span style="font-size:1.2em;font-weight:600">otification </span><span style="font-size:1.2em;font-weight:600">Sound and I</span><span style="font-size:1.2em;font-weight:600">cons</span>
</h3>
<p>
  <span>In order to change the sound and icons of the notifications, replace the sound and icons in the folder Assets/Plugins/Android/Notifications/res.&nbsp;</span>
</p>
<p>
  <span><strong>Note</strong>: The Notification channel name and description can be updated through the <strong>strings.xml</strong> file located in the <strong>Assets\Plugins\Android\Notifications\res\values </strong>folder.</span>
</p>
<h3>
  <span>Remove FCM Dependencies</span>
</h3>
<p>
  <span>By default, FCM dependencies are part of the <strong>SDK.</strong> To remove FCM dependencies, go to the <strong>Assets\Plugins\Android </strong>folder and delete the <strong>Notifications</strong> folder.</span>
</p>
<p>
  <span>To</span>&nbsp;add them back after removing<span>, re-import the Unity package.</span>
</p>
<h2>
  <span>iOS</span>
</h2>
<h3>Adding Scripting Define Symbols</h3>
<p>
  In Unity, go to <strong>Player Settings.&nbsp;</strong>In the
  <strong>Other Settings</strong> section, add the
  <span><strong>"COUNTLY_ENABLE_IOS_PUSH"&nbsp; </strong>symbol in </span><strong>Scripting Define Symbols.</strong><strong><img src="/hc/article_attachments/900004317706/Screenshot_2020-10-27_at_4.07.16_PM.png" alt="Screenshot_2020-10-27_at_4.07.16_PM.png"><br></strong>
</p>
<h3>
  <span>APNs Credentials</span>
</h3>
<p>
  <span>The Countly server needs the APNs <strong>Auth Key&nbsp;</strong>to send notifications. To get the APNs <strong>Auth Key&nbsp;</strong>and upload it to the County Server, refer to <a href="https://support.count.ly/hc/en-us/articles/360037753511-iOS-watchOS-tvOS-macOS#setting-up-apns-authentication" target="_self">iOS Documentation.</a></span>
</p>
<h3 id="configuring-ios-app" class="anchor-heading" tabindex="-1">Configuring the iOS app</h3>
<p>
  After exporting the <strong>iOS</strong> project, open the project in
  <strong>Xcode</strong>, and add <strong>Push Notifications</strong> Capability.<img src="/hc/article_attachments/900004294166/Screenshot_2020-10-26_at_3.13.25_PM.png" alt="Screenshot_2020-10-26_at_3.13.25_PM.png">
</p>
<h3>
  <span>Removing the APNs Dependencies</span>
</h3>
<p>
  <span>By default, the <strong>APNs</strong> dependencies are part of the <strong>SDK.</strong> To remove the <strong>APNs</strong> dependencies, go to the <strong>Assets\Plugins </strong>folder and delete the <strong>iOS</strong> folder. Remove the <strong>"COUNTLY_ENABLE_IOS_PUSH"</strong> symbol from <strong>Scripting Define Symbols </strong>in <strong>Player Settings.</strong></span>
</p>
<p>
  <span>To</span>&nbsp;add them back after removing<span>, re-import the Unity package and add back the <strong>"COUNTLY_ENABLE_IOS_PUSH"</strong> symbol.</span>
</p>
