---
layout: post
title: getting started with Vue js part-1
categories: [Vue-js]
tags: [Vue-js,What-Did-I-Learn-Today]
description: getting started with Vue js part-1
fullview: true
comments: true.
---
**Introduction**
Vue (pronounced /vjuː/, like view) is a progressive framework for building user interfaces. Unlike other monolithic frameworks, Vue is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. On the other hand, Vue is also perfectly capable of powering sophisticated Single-Page Applications when used in combination with modern tooling and supporting libraries.

**Data property**
* can be an object or function
* Vue will recursively convert its properties into getter/setters to make it “reactive”
* native objects such as browser API objects and prototype properties are ignored
* After the instance is created, the original data object can be accessed as vm.$data
* Properties that start with _ or $ will not be proxied on the Vue instance because they may conflict with Vue’s internal properties and API methods. You will have to access them as vm.$data._property.
* When defining a component, data must be declared as a function that returns the initial data object
* If required, a deep clone of the original object can be obtained by passing vm.$data through JSON.parse(JSON.stringify(...))
* Note that you should not use an arrow function with the data property (e.g. data: () => { return { a: this.myProp }}). The reason is arrow functions bind the parent context

**Template Syntax**
* Vue compiles the templates into Virtual DOM render functions
* Vue is able to intelligently figure out the minimal number of components to re-render and apply the minimal amount of DOM manipulations when the app state changes.

**Interpolations**
* The double mustaches interprets the data as plain text, not HTML
{% highlight html %}
 Text : example <span>Message: {{ msg }}</span> 
{% endhighlight %}

* v-once directive will not update UI on data change
{% highlight html %}
 <span v-once>This will never change: {{ msg }}</span> 
{% endhighlight %}

* Note that you cannot use v-html to compose template partials, because Vue is not a string-based templating engine
{% highlight html %}
<span v-html="rawHtml"></span>
{% endhighlight %}

**Attributes**
* attribute will not be included in the element if the value of the property is null, undefined or false
{% highlight html %}
<div v-bind:id="campaing_123"></div>
{% endhighlight %}

**JavaScript Expressions**
* binding can only contain one single expression
{% highlight html %}
 <div v-bind:id="'modal-' + id"></div>
{% endhighlight %}

**Directives**
* Directives are special attributes with the v- prefix (bind, if, for, html etc)
* Directive attribute values are expected to be a single JavaScript expression 
{% highlight html %}
 <p v-if="seen">Now you see me</p>
{% endhighlight %}

**Arguments**
* Here href is the argument, which tells the v-bind directive to bind the element’s href attribute to the value of the expression url.
{% highlight html %}
 <a v-bind:href="url"> ... </a>
{% endhighlight %}

**Modifiers**
* .prevent modifier tells the v-on directive to call event.preventDefault() on the triggered event
{% highlight html %}
 <form v-on:submit.prevent="onSubmit"> ... </form>
{% endhighlight %}

**Shorthands**
* Vue.js provides special shorthands for two of the most often used directives, v-bind and v-on
{% highlight html %}
 <a v-bind:href="url"> </a>
 <a :href="url"> ... </a>
 <a v-on:click="doSomething"> ... </a> 
 <a @click="doSomething"> ... </a>
{% endhighlight %}

**Computed Properties**
* dependency relationship is created between the data and the computed fnunction, 
any changes made to fname and lname will reflect fullName

{% highlight html %}
<div id="test">
  <p>First name: "{{ fName }}"</p>
  <p>Last name: "{{ lName }}"</p>
  <p>Full name: "{{ fullName }}"</p>
</div>
{% endhighlight %}

{% highlight js %}
var vm = new Vue({
  el: '#test',
  data: {
    fname: 'Ram',
    lName: 'Bharlia'
  },
  computed: {
    fullName: function () {
      var sef = this;
      return self.fName + ' ' + self.lName;
    }
  }
})
{% endhighlight %}

**Computed Caching vs Methods**
* computed properties are cached based on their dependencies
* A computed property will only re-evaluate when some of its dependencies have changed
* If the dependent property will not change, computed property will return the previously computed result, which is cached.
* use methods, if caching is not required, or it's computation doesn't dependent on VM's data.

**Computed Setter**

{% highlight js %}
computed: {
  fullName: {
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
{% endhighlight %}