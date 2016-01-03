---
layout: post
title:  "AngularJS Filters"
date:   2016-01-01 14:09:16 -0500
categories: blog development
image: /assets/example.png
tworicks: /assets/tworicks.png
currency: /assets/currency.png
---
Angular is a front-end framework that has a lot of built-in functionality available to you so you don't have to re-invent the wheel and can keep pushing forward. This tutorial will demonstrate the power Angular provides for two-way data binding using built-in filters. I made some starter code for this tutorial so we can jump right into using filters after we add a search input. The data is hard coded instead of linking to a JSON file only for ease of use and it is Rick and Morty themed because I am currently obsessed with this show. Ready? Let's get started.

Starter code can be found [here.](https://github.com/brittonwalker/angular_filters)

or

{% highlight console %}
$ git clone git@github.com:brittonwalker/angular_filters.git
{% endhighlight %}

When you open index.html you should see this in the broswer:

<img src="{{ page.image }}" />

Right now we are looking at a list of ten characters from Rick and Morty, each with a name, occupation, and net-worth. Lets go ahead and add a search bar.

{% highlight html %}
<body>
  <!-- Add this div underneathe and it's contents to your index.html file -->
  <div class="search">
    <h1>List of Characters</h1>
    <label>search: </label>
    <input placeholder="Search for characters" autofocus>
  </div>

  <div class="main" ng-controller="MyController">
    <ul class="characterlist">
      <li class="character cf" ng-repeat="item in characters">
        <img ng-src="images/{{'{{item.shortname'}}}}.png" alt="Photo of {{'{{item.name'}}}}" />
        <div class="info">
          <h2>{{'{{item.name'}}}}</h2>
          <h3>{{'{{item.occupation'}}}}</h3>
          <h4>Net Worth: {{'{{item.netWorth'}}}}</h4>
        </div>
      </li>
    </ul>
  </div>
</body>
{% endhighlight %}

We are going to use this search bar to dynamically display the characters according to the filters we assign it using the 'ng-model' directive to bind our input field within the scope of our controller. To do this, we need to first implement the ng-model on our input which we will call "search".

{% highlight html %}
<input ng-model="search" placeholder="Search for characters" autofocus>
{% endhighlight %}

Next, we will give our controller a filter called 'filter' that selects a subset of items from array and returns it as a new array on the existing 'ng-repeat' directive using a pipe.

{% highlight html %}
<li class="character cf" ng-repeat="item in characters | filter: search">
{% endhighlight %}

After you save and refresh your brower, type in the search bar and you will see the content dynamically display based on what you typed into the input. For example, if we type 'rick' we will get 'Rick Sanchez' and 'Tiny Rick'.

<img src="{{ page.tworicks }}" />

This is TWO-WAY BINDING. This is COOL, or as Rick would say "WUBBA LUBBA DUB DUB". This is the power that Angular provides us without us writing much at all.

You can also apply filters to the variables being passed into the view. The basic syntax works like this:

{% highlight html %}
{{"{{ expression | filter " }}}}
{% endhighlight %}

For a real world example, we'll use the 'currency' filter which formats a number as a currency. You can selectively choose or even make a custom currency, otherwise it will use the default symbol for the current local location. Let's update the characters 'Net Worth' to show USD. The filter currency, along with some other filters, allow arguments. By default for me it would print out in USD anyway but you could change this to any currency or even 'pizza' if you wanted to. I also set the 'fractionSize' which is the second argument, to have no decimal places which will also subsequently round the number.

{% highlight html %}
<h4>Net Worth: {{"{{ item.netWorth | currency : "USD$" : 0 " }}}}
{% endhighlight %}

 <img src="{{ page.currency }}" />

 You can also chain filters together by adding another pipe and a filter. To see a list of built-in filter options, checkout the [Angular documentation.](https://docs.angularjs.org/api/ng/filter)

{% highlight html %}
{{"{{ expression | filter | filter2 | filter3 | etc..." }}}}
{% endhighlight %}

Let's go ahead and chain the limitTo and orderBy filter to our controller output. The limitTo function does exactly as it implies, here it will limit the data to what we specify. Let's limit the characters to 4. Here you can just chain the filter on so while we are at it, lets use the orderBy filter on the characters names to display them alphabetically. The code looks like this:

{% highlight html %}
<li class="character cf" ng-repeat="item in characters | filter: search | limitTo: 4 | orderBy:'name' ">
{% endhighlight %}
