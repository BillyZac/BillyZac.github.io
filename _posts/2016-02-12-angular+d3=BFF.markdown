---
layout: post
title:  "Angular + D3 = BFF"
date:   2016-02-12 00:00:00 -0700
categories: angular d3
published: true
---
Wouldn't it be great to just pop an element in the html and have it magically generate a sweet D3 chart? And the chart is based on whatever data we pass into the html attribute?

![Bar chart]({{ site.url }}/assets/bar-chart.png)

{% highlight html %}
<chart data="chartData"></chart>
{% endhighlight %}

Today, dreams become reality as we take a look at how to use the awesome [D3.js library](https://d3js.org/) in an [Angular](https://angularjs.org/) app.

I tried to think up a really simple example to demonstrate the basic principles in a clear way and I came up with a little bar chart that pulls it's data from an Angular controller. Maybe this won't dazzle the pants off your eyeballs, but hopefully it provides the basic building blocks for more ambitious Angular/D3 projects.

## Define the data set
In our controller, we define the data:

{% highlight javascript %}
// main.controller.js

angular.module('app')
.controller('MainController', MainController)

function MainController($scope) {
    $scope.chartData = [5, 10, 15, 20, 25, 17, 3, 46]
}

{% endhighlight %}

## Define our new <chart> element
In angular, we can create our own html elements using custom directives.

First, we register the directive:
{% highlight javascript %}
// chart.directive.js
angular.module('app')
.directive('chart', chart)
{% endhighlight %}

`chart` is a function that describes our new element. All it does is return an object with a bunch of properties. These properties define a series of options that tell Angular how to build our custom directive:
{% highlight javascript %}
function chart() {
  return {
    restrict: 'E',
    template: '<div class="chart"></div>',
    scope: { data: '=' },
    link: chartLink
  }
}
{% endhighlight %}

Let's look at what these options do.

`restrict: 'E'` says "Make this an html element". This is what allows us to write `<chart>` in our html.

`template` defines the html that will end up on the page.

`scope: { data: '=' }` This line is very important. This tells Angular that we're interested only in the `data` attribute. Without this, we expose the entire $scope of the controller to our custom directive. This would not be good! We want to prevent the controller and the directive from modifying each other's data except in ways we explicitly allow.

By defining `scope`, we're declaring what's allowed to go in and out of our directive. No other communication is allowed.

We need to do this because our `<chart>` makes use of D3, which manipulates the DOM. Angular also manipulates the DOM. If we don't take care to isolate the scope, Angular and D3 could get into a big confusing mess of a fight.

Finally, we have `link: chartLink`. The link option is [typically used when we want a custom directive to modify the DOM](https://docs.angularjs.org/guide/directive#creating-a-directive-that-manipulates-the-dom). The value assigned to the link property is [a function that deals with DOM listeners and DOM manipulation](https://docs.angularjs.org/api/ng/service/$compile#-link-). In our case, we're telling Angular to look to the `chartLink` function for the implementation details of our `<chart>`. So let's write that function.

## Implementing the chart
Here's where we actually write the D3 code that generates the chart.

{% highlight javascript %}
function chartLink($scope, $element, $attr) {
  var element =
    d3.select($element[0])
      .append("svg")
      .attr("width", 300)
      .attr("height", 300)

    element.selectAll("rect")
      .data($scope.data)
      .enter()
      .append("rect")
      .attr({
        x: function(d, i) {return i * 300 / $scope.data.length},
        y: function(d) {return 300 - (d * 4) + 3},
        width: 300 / $scope.data.length - 5,
        height: function(d) {return d * 4}
      })
}
{% endhighlight %}

Most of this is basic D3 stuff, but there are a few lines that could be a little tricky:

The part where we select the element might look funny: `d3.select($element[0])`. Here, we're telling D3 that we want to attach the chart to the element we've defined in the custom directive. "Hey D3, put the chart in the `<chart>`."

Another notable line is the one where we make use of the data that was defined on the controller: `.data($scope.data)`. Remember how we defined the scope that is available to our custom directive? We're making use of that here.

## Reusability

Now we have our own custom html tag that we can use over and over, passing it whatever data we want. The html is clean and readable, with all of the messy implementation details hidden away.

![Three bar charts]({{ site.url }}/assets/three-bar-charts.png)

{% highlight html %}
<chart data="chartData1"></chart>
<chart data="chartData2"></chart>
<chart data="chartData3"></chart>
{% endhighlight %}

## Full example code

See it all together [on GitHub](https://github.com/BillyZac/Angular-D3-BFF).
