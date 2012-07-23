---
layout: post
title: "JavaScript new Regex() vs //"
date: 2012-07-23 11:53
comments: true
categories: 
- JavaScript
- regular expression
- regexp
---

JavaScript knows two ways to construct [regular expressions][1]. One
way is the constructor of the [RegExp][2] object. E.g.

{% codeblock lang:javascript %}
var r = new RegExp('abra');
if ("abracadabra".match(r)) {
    console.log("positive match");
}
{% endcodeblock %}

The other way is via a [literal regular expression][2] syntax. E.g.

{% codeblock lang:javascript %}
var r = /abra/;
if ("abracadabra".match(r)) {
    console.log("positive match");
}
{% endcodeblock %}

I recently developed a strong preference for `new RegExp()`. The reason
was found in a bug I recently solved with a coworker.

While working on a [Grails][3] application we were sending a Model to
populate a View. On this View a property of the model was used to
create a regular expression with. This was achieved with code simlar
to the example below

{% codeblock lang:javascript Potentially problematic code %}
(function(){
    var Account = {};

    ...

    Account.validator = createValidatorFrom(/${model.regexp}/);
    
    ...

    window.Account = Account;
})()
{% endcodeblock %}

In this case `model.regexp` is used to create a regular expression
that is passed into the `createValidatorFrom` JavaScript method. So
far so good.

But what happens if the `model.regexp` is `null`? Then the
`${model.regexp}` will collapse to the empty string rending the
JavaScript snippet invalid. The reason being that the literal regular
expression accidently got turned into a single line comment hiding the
closing bracket.

{% codeblock lang:javascript Regular expression literal or single line comment? %}
(function(){
    var Account = {};

    ...

    Account.validator = createValidatorFrom(//);
    
    ...

    window.Account = Account;
})()
{% endcodeblock %}

If the `new RegExp()` constructor was used this would not become a
problem. The constructor accepts a string. No slash-delimiter could be
turned into a single line comment by accident.

Having said that, care should still be taken when working with regular
expressions. Character classes like `\d` or `\s` tend to be
interpreted in JavaScript string, causing problems of their own.

[1]: http://en.wikipedia.org/wiki/Regular_expression "Wikipedia on Regular Expressions."
[2]: http://www.w3schools.com/jsref/jsref_obj_regexp.asp "W3Schools on RegExp"
[3]: http://grails.org/ "Grails Homepage"