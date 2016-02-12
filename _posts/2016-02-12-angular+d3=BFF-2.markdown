---
layout: post
title:  "Angular + D3 = BFF, part 2"
date:   2016-02-12 00:00:01 -0700
categories: angular d3
published: true
---
In this post, we'll look at how to use Angular's two-way binding to drive the display of a D3 visualization.

(This post builds on concepts described in [my previous post on Angular and D3](http://billyzac.github.io/angular/d3/2016/02/12/angular+d3=BFF.html).)

We'll use a super simple example to demonstrate. Here we have a custom directive that draws a circle. The size of the circle is determined by a number the user types in the input field:
![Circle directive]({{ site.url }}/assets/circle-directive.png)

{% highlight html %}
  <input name="circleSize" ng-model="circleSize">
  <circle circle-size="circleSize"></circle>
{% endhighlight %}

In ordinary Angular, we don't have to worry about syncing up the user's input with DOM manipulation. That's part of the magic of Angular. But since we're using D3 within a custom directive, we need to do a little extra work to make sure the signal gets through.

## Watching the scope

We need to watch the _circleSize_, and whenever it changes, we need to update the display of the circle. Ordinarily, Angular's digest cycle would take care of this watching and updating for us. But our D3 code is cut off from the normal Angular digest cycle, so we need to handle it manually. Luckily, Angular gives us a way to watch for changes on the scope: `$scope.$watch`. We use it like this:

{% highlight javascript %}
function update($scope) {
  $scope.$watch('circleSize', function(newVal, oldVal) {
    if(newVal) {
      d3.select('.circle-shape')
        .attr({ r: $scope.circleSize / 2 })
    }
  })
{% endhighlight %}

Whenever the user updates 'circleSize', this function notices the change and updates the circle's radius accordingly.

## The link function

`update` is called within the `link` function, one of the options we set on the custom directive. The `link` function is where Angular recommends that we set DOM listeners and do DOM manipulation, so it's the perfect place to do our business with the circle radius. When we call `update`, we need to pass it the directive's $scope so that it has access to the circleSize attribute:

{% highlight javascript %}
update($scope)
{% endhighlight %}

The end result: a tiny little D3 visualization that responds dynamically to user input!

## Full example code

See it all together [on GitHub](https://github.com/BillyZac/Angular-D3-BFF).
