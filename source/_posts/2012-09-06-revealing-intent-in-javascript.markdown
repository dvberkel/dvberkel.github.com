---
layout: post
title: "Revealing Intent in JavaScript"
date: 2012-09-06 08:37
comments: false
categories:
- JavaScript
- Craftsmanship
---

[Uncle Bob][1]'s book [Clean Code][2] has a chapter called Meaningfull
Names. In this chapter Uncle Bob offers advice, tips, tricks and ideas
how to reveal the intention of the code. There is web of pages on the
content creation wiki [C2][3] with a similiar focus. The page
[Intention Revealing Names][4] is a good starting place.

The goal of intention revealing code is to aid in [Hermeneutics][5], a
fancy word for 

{% blockquote Wikipedia http://en.wikipedia.org/wiki/Hermeneutics Hermeneutics %}
the art and science of text interpretations.
{% endblockquote %}

The motivation behind intention revealing code is the insight that the
amount of code-reading far outweighs the amount of code-writing. Jeff
Atwood wrote a nice blog about the issue: [When understanding means Rewriting][6].

I recently developed a style which communicates the intent of code
very clearly. In this blog I would like to share my insights, hoping
that others may benefit. Lets start with an example.

{% codeblock lang:javascript Example of intention revealing code %}
var an_array = ['a', 'b', 'c', 'd'];

swap('b').and('d').in(an_array);
{% endcodeblock %}

I would wager that the intent of the above code is clear. In my
opinion the code is very readable, focussing on the What instead of
the How. It uses a [fluent interface][7], as introduced by Martin
Fowler. A possible implementation is given below.

{% codeblock lang:javascript Possible implementation %}
var swap = function(this_element) {
    return {
        and : function(that_element) {
            return {
                in : function(an_array) {
                    var index_of = function(element){ return an_array.indexOf(element); };
                    place(this_element).at(index_of(that_element)).in(an_array);
                    place(that_element).at(index_of(this_element)).in(an_array);
                }
            };
        }
    };
};

var place = function(element) {
    return {
        at : function(index) {
            return {
                in : function(an_array) {
                    an_array[index] = element;
                }
            };
        }
    };
};
{% endcodeblock %}

[1]: https://sites.google.com/site/unclebobconsultingllc/ "Robert C. Martin's homepage"
[2]: http://books.google.nl/books?id=dwSfGQAACAAJ&dq=clean+code&source=bl&ots=YW1kw3CKTZ&sig=yjMaggeNUevDFvvgHaK_Ueyxr4s&hl=en&sa=X&ei=80RIUOfXNIayhAfZpIGgDA&redir_esc=y "Clean Code on google books"
[3]: c2.org "Content Creation Wik on Patterns in Software Development"
[4]: http://c2.com/cgi/wiki?IntentionRevealingNames "C2 on Intention Revealing Names"
[5]: http://en.wikipedia.org/wiki/Hermeneutics "Wikipedia on Hermeneutics"
[6]: http://www.codinghorror.com/blog/2006/09/when-understanding-means-rewriting.html "When understanding means Rewriting on Coding Horror"
[7]: http://martinfowler.com/bliki/FluentInterface.html "Martin Fowler on Fleunt Interface"

