!SLIDE

#delegate :wtf, :to => :omg#
##Refactoring Legacy Applications##

<div class="personal-info">
<ul>
<li>Keith Gaddis</li>
<li>Inductive Applications
<li>@karmajunkie</li>
<li>keith.gaddis@advanceclaim.com</li>
</ul>
</div>

!SLIDE transition=fade bullets incremental

#Let's start with some questions
* Who creates test suites for apps religiously?
* Who has never written an automated test suite?
* Who has worked on projects at any point in the past few years that made you want to cause physical harm to the guy who was there before you?

!SLIDE bullets incremental

#Problem: Rails is maturing
* Released in July 2004, 1.0 in 2005, 2.0 in 2007, 3.0 in 2010
* Widespread mainstream adoption => 
* Legacy code

!SLIDE bullets incremental
#Legacy Code
* Version problem
* Maintenance problem
* Apprentice problem

!SLIDE bullets incremental
#Results
* Poorly constructed apps
* Lack of test suite
* Lots of dead code 
* (Rinse, repeat)

!SLIDE bullets incremental
#Results for Customers
* Buggy software
* Slow release cycles
* Fear of updates

!SLIDE bullets incremental
#Results for devs
* Fear of changes
* Inability to fully appreciate effects of change
* Frustration while working
* Long hours

!SLIDE transition=toss
#You should not fear your software!

!SLIDE center
#The Situation
![The Situation](the_situation.jpg)

!SLIDE transition=toss

<div class="story">
<p>You've been on the job about two weeks.</p>
<p>
You're the guy they hired a few weeks after the architect of the application bailed on the company. 
</p>
<p>
Very little documentation.
</p>

<p>
Bugs are popping up out of nowhere.  Customers are increasingly unhappy.
</p>

<p>
Management is frantic. And looking at you.
</p>
<p>
(Also, someone took the last Red Bull.)
</p>
</div>

!SLIDE
# Assumption 
## Both you and management want to fix the problem, not cover it up

!SLIDE bullets incremental
#Understand your priorities: S.T.O.P.

* S – Stop
* T — Think
* O — Observe
* P — Plan

!SLIDE bullets incremental
#Stop
* When you find yourself in a hole, stop digging.
* Communicate with stakeholders: Product manager, Executive team, Customers
* Get in front of issues
* Most important: Set expectations for the refactoring process

!SLIDE bullets incremental
#Think
* Assess problem areas
* Develop intelligence
* Safety first!

!SLIDE bullets incremental
#Think: Assess
##Browse the code
* routes
* ApplicationController
* model/controller count
* rake stats

!SLIDE metric-fu-roodi transition=toss
#Think: Assess
##Metrics with metric_fu/metrical
<img alt="metrics" src="/image/presentation/metrics_roodi.png">

.description `metric_fu` is a gem that will run an extensive suite of metrics on your app, giving you information about hotspots, complex code, and code smells in your app.

.description Most importantly, it can log these metrics over time, giving you a benchmark for your progress.

.description Metrical is a command-line interface: `gem install metrical`

.description Ryan Bates metrics screencast: <http://railscasts.com/episodes/252-metrics-metrics-metrics>

!SLIDE bullets incremental
#Think: Develop intelligence
## Know about your customers problems
## They complain, but not always to you.
* Exception notifier
* Hoptoad/Errbit
* New Relic RPM
* Scout

!SLIDE smbullets incremental
#Think: Safety
##Warning: ruffled feathers ahead
* An extensive test suite is your safety net
* You are not a good enough software developer to work without a net<br>(*and neither is the guy coming behind you*)
* Delivering features without tests is the same as buying them on credit
* If you have a *really good* credit rating your interest is low
* *You are still paying interest*

!SLIDE bullets incremental
#Think: Safety
## Two good places to start testing
## opposite ends of spectrum
* Unit tests
* Integration tests

!SLIDE bullets incremental
#Unit tests
* Stand alone
* Fewer interactions
* Limit to "weird parts"
* Limited value

!SLIDE bullets incremental
#Integration tests
* Cucumber is the best choice
* *yes, this is a controversial statement*
* Features are documentation
* Accessible language means you can bring in stakeholders
* BDD approach lets you get away from your really messy internals and focus on the interface

!SLIDE smbullets incremental
#Behavior Driven Development
* <blockquote>"BDD is a second-generation, outside-in, pull-based, multiple-stakeholder, multiple-scale, high-automation, agile methodology. It describes a cycle of interactions with well-defined outputs, resulting in the delivery of working, tested software that matters."</blockquote>
* WTF does that even mean?
* You don't focus on the code, you focus on the interface.
* This is important, because your code probably sucks and is going to be really hard to test.

!SLIDE code 
##login.feature
<pre><code>
Feature: Logging in
  In order to allow users to use our site and drive ad revenue
  As a registered user
  I want to log in with my username and password

  Background:
    Given the following user exists:
      | username | 
      | keith    |
    When I go to the home page
     And I fill in "Username" with "keith"
     And I fill in "Password" with "password"
     And I press "Log in"
    Then I should see "Welcome to the flip side"
</code></pre>

!SLIDE code 
##spec/factories.rb
<pre><code>
Factory.define(:user) do |f|
  f.sequence(:username) {|i| "user_#{"%04d" % i}"}
  f.password "password"
end

Factory(:user).login # => "user_0001"
</code></pre>

!SLIDE smbullets incremental
##Other testing tools to use
* `steak` (alternative to cucumber)<br>*if you're trying to involve stakeholders, this is a poor choice* 
* `capybara` — interact with the web page
* `factory_girl` — create your data
* `webmock` — mock out external service dependencies
* `email_spec` — testing email functions
* Selenium/Webdriver — test in a browser (allow AJAX testing)

!SLIDE bullets observe incremental
#Observe
* As you're making changes, you have to keep an eye on the running application
* *hint: you don't have to wait until you're making changes to do this*

!SLIDE bullets incremental
#Observe: Build
* You've got tests, now set up a Continuous Integration server <br>*Jenkins (neé Hudson), CruiseControl, CI Joe*
* Create a metrics build to keep an eye on your hotspots over time

!SLIDE smbullets incremental 
#Observe: Monitor
* Same toolsets as you use to assess your application
* *RPM, Scout, Hoptoad/Errbit*
* Jump on exceptions with a vengeance
* Never fix a bug without a test—use the opportunity to build your test suite out
* Remember goal: increase stability of software
* Prioritize user experience

!SLIDE 
##Now, the fun part##
#Plan#

!SLIDE transition=toss smbullets incremental
#Plan
* Figure out what areas to tackle in order of priority
* Prefer to prioritize defects over features
* Go for the quick wins and low-hanging fruit<br>*hint: if you can grep it, you can refactor it*
* Look for areas you can pull into gems
* Decide on coding standards, and stick to them
* Use version control religiously
* Create a deployment process

!SLIDE smbullets incremental
#Additional thoughts to keep in mind
* This can be an emotional process
* Difficult to know where to start
* Demoralizing
* Try to find ways to mix it up
* Ex: Implement a small, high-value, well-defined, self-contained feature<br>Use a cool technology (appropriately)<br>Work in pairs and switch frequently
* Recognize that ***it gets easier the more you do it.***

!SLIDE thanks
#Thanks
* Datran/Otherinbox for hosting us
* Austin on Rails for giving me a soapbox
* Inductive Applications 
<div class="personal-info">
<ul>
<li>Keith Gaddis</li>
<li>Inductive Applications
<li>@karmajunkie</li>
<li>keith.gaddis@advanceclaim.com</li>
<li>Presentation available at <a href="http://github.com/karmajunkie/wtfomg">http://github.com/karmajunkie/wtfomg</a></li>
</ul>
</div>
