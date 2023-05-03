Download Link: https://assignmentchef.com/product/solved-cs2030s-lab-2-cruise-loaders-2
<br>
<h1></h1>

<h2>Topic Coverage</h2>

<ul>

 <li>Inheritance</li>

 <li>Polymorphism</li>

 <li>Method Overriding</li>

 <li><a href="https://www.comp.nus.edu.sg/~cs2030/style/" target="_blank" rel="noopener">CS2030 Java Style Guide</a></li>

</ul>

<h2>Problem Description</h2>

The Kent Ridge Cruise Centre has just opened and you are required to design a program to decide how many loaders to buy based on a single-day cruise schedule.

<h3>Cruises</h3>

Each cruise has four attributes:

<ul>

 <li>a unique string identifier, e.g. <tt>S1234</tt></li>

 <li>a time of arrival in HHMM format, e.g. <tt>2359</tt> denoting the cruise arriving at 11:59PM on that day</li>

 <li>the time needed to serve the cruise (in minutes), and</li>

 <li>the number of loaders needed to serve the cruise.</li>

</ul>

Every cruise must be served by loaders immediately upon arrival.

There are two types of cruises:

<ul>

 <li><code>SmallCruise</code>:

  <ul>

   <li>has an identifier that starts with an uppercase character <tt>S</tt>;</li>

   <li>takes a fixed 30 minutes for a loader to fully load;</li>

   <li>requires only one loader for it to be fully served;</li>

  </ul></li>

 <li><code>BigCruise</code>:

  <ul>

   <li>has an identifier that starts with an uppercase character <tt>B</tt>;</li>

   <li>takes one minute to serve every 50 passengers;</li>

   <li>requires one loader per every 40 meters in length of the cruise (or part thereof) to fully load</li>

  </ul></li>

</ul>

<h3>Loaders</h3>

Loaders have to be purchased to serve cruises. Each loader comprises two attributes:

<ul>

 <li>a unique integer identifier</li>

 <li>the cruise that it is currently serving</li>

</ul>

A loader will serve a cruise as soon as it arrives, and continues to do so until the service time has elapsed (i.e. it cannot serve a cruise while in the midst of serving another one).

For example, if an incoming cruise arrives at 12PM, requires two loaders, and 60 min for it to be fully served, then, at 12PM, there must be two vacant loaders. These two loaders will serve the cruise from 12PM – 1PM. They can only serve another cruise from 1PM onwards.

<h2>Task</h2>

Your task is to determine the loader allocation schedule using the following steps:

<ul>

 <li>For each cruise, check through the inventory of existing loaders, starting from the loader first purchased, and so on;</li>

 <li>The first (or first few) loaders available will be used to serve the cruise;</li>

 <li>If there are not enough loaders, purchase new one(s) to serve the cruise.</li>

</ul>

Take note of the following assumptions:

<ul>

 <li>Input cruises are presented chronologically by arrival time.</li>

 <li>There can be up to 30 cruises in one day.</li>

 <li>The number of loaders servicing a cruise will not exceed 9.</li>

 <li>There are no duplicate cruises.</li>

 <li>All cruises will arrive and be completely served within a single day..</li>

</ul>

Although this problem can be implemented procedurally, you are to model your solution using an object-oriented approach instead.

This task is divided into five levels. You need to complete ALL levels.

<table border="1" cellpadding="10">

 <tbody>

  <tr>

   <td><h2>Level 1</h2><h2>Represent a Cruise</h2>Design an <em>immutable</em> <code>Cruise</code> class to represent a cruise having a unique identifier string, the time of arrival as an integer, the number of loaders required to load the cruise as an integer, and the service time in minutes as an integer.<pre>class Cruise {    private final String identifier;    private final int arrivalTime;    private final int numOfLoader;    private final int serviceTime;    ...}</pre>Note that the time of arrival is in <tt>HHMM</tt> format. Specifically, <tt>0</tt> or <tt>0000</tt> refers to 00:00 (12AM), <tt>30</tt> or <tt>0030</tt> refers to 00:30 (12:30AM), and <tt>130</tt> or <tt>0130</tt> refers to 01:30 (1:30AM).Implement a <code>getServiceCompletionTime</code> method, which returns the time the service completes (in number of minutes) since midnight, and a <code>getArrivalTime</code> method, which returns the arrival time (in number of minutes) since midnight.For example, if the cruise arrives at 12PM (noon time), the arrival time is (12 * 60) = 720; the service completion time is 12:30PM, which is 750 minutes since midnight, i.e. (12 * 60) + 30 = 750.In addition, implement a <code>getNumOfLoadersRequired</code> method, which returns the number of loaders required to load the cruise.A string representation of a cruise is in the form:<pre><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d5b6a7a0bca6b09c91959d9d9898">[email protected]</a></pre>The <code>%0Xd</code> format specifier might be of use to you, where the integer will be represented by an X-digit zero-padded number. For instance,<pre>String.format("%04d", 20);</pre>would return the string <code>0020</code>.<pre>jshell&gt; /open Cruise.javajshell&gt; new Cruise("A1234", 0, 2, 30)$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="9edfafacadaadeaeaeaeae">[email protected]</a>jshell&gt; new Cruise("A2345", 30, 2, 30)$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f5b4c7c6c1c0b5c5c5c6c5">[email protected]</a>jshell&gt; new Cruise("A3456", 130, 2, 30)$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0c4d3f38393a4c3c3d3f3c">[email protected]</a>jshell&gt; new Cruise("A3456", 130, 2, 30).getArrivalTime()$.. ==&gt; 90jshell&gt; new Cruise("A3456", 130, 2, 30).getNumOfLoadersRequired()$.. ==&gt; 2jshell&gt; new Cruise("A3456", 130, 5, 30).getNumOfLoadersRequired()$.. ==&gt; 5jshell&gt; new Cruise("A1234", 0, 2, 30).getServiceCompletionTime()$.. ==&gt; 30jshell&gt; new Cruise("A1234", 0, 2, 45).getServiceCompletionTime()$.. ==&gt; 45jshell&gt; new Cruise("CS2030", 1200, 2, 100).getServiceCompletionTime()$.. ==&gt; 820jshell&gt; new Cruise("D1010", 2329, 2, 30).getServiceCompletionTime()$.. ==&gt; 1439jshell&gt; /exit</pre>Check the format correctness of the output by running the following on the command line:<pre>$ javac *.java$ jshell -q &lt; test1.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

  <tr>

   <td><h2>Level 2</h2><h2>Use Loaders to serve Cruises</h2>Design an <em>immutable</em> <code>Loader</code> class to serve a cruise.<pre>class Loader {    private final int identifier;    private final Cruise cruise;    ...}</pre>Include the following in the <tt>Loader</tt> class:

    <ul>

     <li>a constructor that takes in an integer denoting its unique identifier, as well as the first cruise that it serves.</li>

     <li>a <code>canServe(Cruise)</code> method that returns <code>true</code> if the loader is available to serve the given cruise, or <code>false</code> otherwise.</li>

     <li>a <code>serve(Cruise)</code> method to serve a given cruise. If the loader is available, the method returns the loader serving this cruise; otherwise the existing loader is returned</li>

     <li>the methods <tt>getIdentifier()</tt> and <tt>getNextAvailableTime()</tt> to return the loader’s identifier, and the next available time for service.</li>

    </ul>The string representation of each Loader is in the form:<pre>Loader ID serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e48796918d9781ada0a48796918d97818596968d928588908d8981">[email protected]</a></pre><pre>jshell&gt; /open Cruise.javajshell&gt; /open Loader.javajshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2d6c1c1f1e196d1d1d1d1d">[email protected]</a>jshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).getIdentifier()$.. ==&gt; 1jshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).getNextAvailableTime()$.. ==&gt; 30jshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).canServe(new Cruise("A2345", 30, 1, 30))$.. ==&gt; truejshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).serve(new Cruise("A2345", 30, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6627545552532656565556">[email protected]</a>jshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).serve(new Cruise("A2345", 30, 1, 30)).getNextAvailableTime()$.. ==&gt; 60jshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).canServe(new Cruise("A2345", 10, 1, 30))$.. ==&gt; falsejshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).serve(new Cruise("A2345", 10, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="98d9a9aaabacd8a8a8a8a8">[email protected]</a>jshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).serve(new Cruise("A2345", 10, 1, 30)).getNextAvailableTime()$.. ==&gt; 30jshell&gt; new Loader(1, new Cruise("A1234", 0, 1, 30)).serve(new Cruise("A2345", 10, 1, 30)).getIdentifier()$.. ==&gt; 1jshell&gt; /exit</pre>Check the format correctness of the output by running the following on the command line:<pre>$ javac *.java$ jshell -q &lt; test2.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

  <tr>

   <td><h2>Level 3</h2><h2>Represent Small Cruises</h2>Design the <code>SmallCruise</code> class. The arguments of the constructor are its identifier, and time of arrival. Note that you should not need to change your <code>Loader</code> class if you have implemented it properly.<pre>jshell&gt; /open Loader.javajshell&gt; /open Cruise.javajshell&gt; /open SmallCruise.javajshell&gt; new SmallCruise("S0001", 0).getArrivalTime()$.. ==&gt; 0jshell&gt; new SmallCruise("S0001", 0).getServiceCompletionTime()$.. ==&gt; 30jshell&gt; new SmallCruise("S0001", 0).getNumOfLoadersRequired()$.. ==&gt; 1jshell&gt; (Cruise) new SmallCruise("S0123", 1220)$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d182e1e0e3e291e0e3e3e1">[email protected]</a>jshell&gt; new Loader(1, new SmallCruise("S1245", 2330))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e0b3d1d2d4d5a0d2d3d3d0">[email protected]</a>jshell&gt; new Loader(1, new SmallCruise("S1245", 2330)).canServe(new SmallCruise("S2345", 2359))$.. ==&gt; falsejshell&gt; new Loader(1, new SmallCruise("S1245", 2330)).serve(new SmallCruise("S2345", 2359))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1241232026275220212122">[email protected]</a>jshell&gt; new Loader(1, new SmallCruise("S2030", 0))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="98cbaaa8aba8d8a8a8a8a8">[email protected]</a>jshell&gt; /exit</pre>Check the format correctness of the output by running the following on the command line:<pre>$ javac *.java$ jshell -q &lt; test3.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre> </td>

  </tr>

  <tr>

   <td><h2>Level 4</h2><h2>Represent Big Cruises</h2>Design the <code>BigCruise</code> class. The arguments of the constructor are its identifier, time of arrival, the length of the cruise, and number of passengers, in that order.<pre>jshell&gt; /open Loader.javajshell&gt; /open Cruise.javajshell&gt; /open SmallCruise.javajshell&gt; /open BigCruise.javajshell&gt; Cruise b = new BigCruise("B0001", 0, 70, 3000)jshell&gt; b.getArrivalTime()$.. ==&gt; 0jshell&gt; b.getServiceCompletionTime()$.. ==&gt; 60jshell&gt; b.getNumOfLoadersRequired()$.. ==&gt; 2jshell&gt; new Loader(1, b).serve(b)$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6f2d5f5f5f5e2f5f5f5f5f">[email protected]</a>jshell&gt; new Loader(1, b).serve(b).getNextAvailableTime()$.. ==&gt; 60jshell&gt; new Loader(2, b)$.. ==&gt; Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6b295b5b5b5a2b5b5b5b5b">[email protected]</a>jshell&gt; new Loader(3, b)$.. ==&gt; Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="084a383838394838383838">[email protected]</a>jshell&gt; new Loader(4, new BigCruise("B2345", 0, 30, 1450)).serve(new SmallCruise("S0000", 29))$.. ==&gt; Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c093f0f0f0f080f0f0f2f9">[email protected]</a>jshell&gt; new Loader(5, new BigCruise("B3456", 0, 75, 1510)).serve(new SmallCruise("S0001", 30))$.. ==&gt; Loader 5 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7436474041423444444444">[email protected]</a>jshell&gt; /exit</pre>Check the format correctness of the output by running the following on the command line:<pre>$ javac *.java$ jshell -q &lt; test4.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre> </td>

  </tr>

  <tr>

   <td><h2>Level 5</h2><h2>Output the loader allocation schedule</h2>Write a method <tt>serveCruises(Cruise[])</tt> that takes in an array of cruises, and outputs the allocation schedule of the loaders required to service all the cruises. Save the method in the file <tt>level5.jsh</tt>.You may assume that there are at most 30 cruises in one day, and the number of loaders servicing a cruise will not exceed 9.<pre>jshell&gt; /open Cruise.javajshell&gt; /open SmallCruise.javajshell&gt; /open BigCruise.javajshell&gt; /open Loader.javajshell&gt; /open level5.jshjshell&gt; Cruise[] cruises = {   ...&gt;     new SmallCruise("S1111", 1300)}jshell&gt; serveCruises(cruises);Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0c5f3d3d3d3d4c3d3f3c3c">[email protected]</a>jshell&gt; Cruise[] cruises = {   ...&gt;     new BigCruise("B1111", 1300, 80, 3000),   ...&gt;     new SmallCruise("S1111", 1359),    ...&gt;     new SmallCruise("S1112", 1400),    ...&gt;     new SmallCruise("S1113", 1429)}jshell&gt; serveCruises(cruises);Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="90d2a1a1a1a1d0a1a3a0a0">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0c4e3d3d3d3d4c3d3f3c3c">[email protected]</a>Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="683b5959595928595b5d51">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5407656565661465606464">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cd9efcfcfcfe8dfcf9fff4">[email protected]</a>jshell&gt; Cruise[] cruises = {   ...&gt;     new SmallCruise("S1111", 900),    ...&gt;     new BigCruise("B1112", 901, 100, 1),   ...&gt;     new BigCruise("B1113", 902, 20, 4500),   ...&gt;     new SmallCruise("S2030", 1031),    ...&gt;     new BigCruise("B0001", 1100, 30, 1500),   ...&gt;     new SmallCruise("S0001", 1130)}jshell&gt; serveCruises(cruises);Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3063010101017000090000">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6022515151522050595051">[email protected]</a>Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="25671414141765151c1514">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0d4f3c3c3c3f4d3d343d3c">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b6f487878785f6868f8684">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5506676566651564656664">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0143313131304130303131">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d380e3e3e3e293e2e2e0e3">[email protected]</a>jshell&gt; /exit</pre>Check the format correctness of the output by running the following on the command line:<pre>$ javac *.java$ jshell -q &lt; test5.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

  <tr>

   <td><h2>Level 6</h2><h2>Recycled Loaders</h2>The objective of this level is to determine whether your current implementation can be easily extended with minimal modification to the clientThe Cruise Centre has just introduced a new eco-friendly policy in an effort to go green. Their policy states that every third loader that is purchased must be made of recycled materials (referred to as recycled loaders). These recycled loaders will go through a 60-minute long maintenance after every service. It is unable to serve any cruise during this period.For example, if a recycled loader serves a <code>SmallCruise</code> that arrives at 12:30PM, then the next time the loader can serve another <code>Cruise</code> is 2PM (30min + 60min after 12:30PM).By modifying <tt>level5.jsh</tt>, incorporate the above with minimal modifications to the file <tt>level6.jsh</tt>.<pre>jshell&gt; /open Cruise.javajshell&gt; /open SmallCruise.javajshell&gt; /open BigCruise.javajshell&gt; /open Loader.javajshell&gt; /open RecycledLoader.javajshell&gt; /open level6.jshjshell&gt; Cruise[] cruises = {   ...&gt;     new BigCruise("B1111", 0, 60, 1500),   ...&gt;     new SmallCruise("S1112", 0),    ...&gt;     new BigCruise("B1113", 30, 100, 1500),   ...&gt;     new BigCruise("B1114", 100, 100, 1500),   ...&gt;     new BigCruise("B1115", 130, 100, 1500),   ...&gt;     new BigCruise("B1116", 200, 100, 1500)   ...&gt; }jshell&gt; serveCruises(cruises);Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2a681b1b1b1b6a1a1a1a1a">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="9fddaeaeaeaedfafafafaf">[email protected]</a>Recycled Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c89bf9f9f9fa88f8f8f8f8">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3674070707057606060506">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="83c1b2b2b2b0c3b3b3b0b3">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f6b4c7c7c7c5b6c6c6c5c6">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="9cdeadadada8dcacadacac">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1c5e2d2d2d285c2c2d2c2c">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c785f6f6f6f387f7f6f7f7">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7d3f4c4c4c483d4d4c4e4d">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2e6c1f1f1f1b6e1e1f1d1e">[email protected]</a>Recycled Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5f1d6e6e6e6a1f6f6e6c6f">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="ecaedddddddaacdcdedcdc">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="99dba8a8a8afd9a9aba9a9">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="682a5959595e28585a5858">[email protected]</a>jshell&gt; /exit</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

 </tbody>

</table>