title: full backend mock with AngularJS
date: 2013-10-19 15:35:42
tags: angularjs, mock, backend, javascript, html5, plunkr, gist
---

Many times when developing a multi-layered web application you had to wait for the backend APIs or for the database to work.. or for _something else_, in order to show your work.  
If you are using AngularJS, the good news is that you have a workaround to make things _appear_ to work :)

### ngMockE2E
First thing first, the module to simulate a true backend is [ngMockE2E][ngmocke2e] and **not** [ngMock][ngmock].  
The former only mocks the `$httpBackend` service, and auto-responds to requests, that's exactly why we want to use it!

A little example about how it works:

{% gist 7056200 mock.js %}
[view full gist here][gist-step1]

[plunker working demo][plunker-step1]

As you can see ngMock require a synchronous return object, there's no workaround for this, it's desiged for testing, and for simplicity.  
Although you can access/write [localStorage][localstorage], [SQlite][sqlite] or [indexedDB][indexeddb] synchronously, so you've many opportunities to manipulate data.  

##### Some outdated table about browser support
{% img http://blog.elendev.com/wp-content/uploads/2011/12/localStorageBrowserSupport.png thanks to elendev.com %}

While i don't see many limits with this, sometimes you'll want to simulate a true backend, with the classic delays that suits a true AJAX response.  
They were invented to be _slow_ after all, otherwise they would be synchronous

### The next level, delay.

ngMock of course isn't thinked with delay support, some [brilliant guy][endlesslink] made a decorator to delay all the responses, but i wanted to do more.  
In a public project that i'm developing, i needed xhr request abortion, that of course isn't supported in mocking; in other words every request made gets completed, no matter what  

But we can simulate _angular.js_ behaviour with a simple `$http` interceptor:

{% gist 7056188 mock-delay.js %}

Possibilities here are vast, randomize timeout, simulate packet drops, authentication simulation, many ideas can take life with just those simple pieces of code.  

[plunker live demo with random delay][plunker-step2]

I hope some other hackers love this flexibility as much as i do, happy coding!

[ngmocke2e]: http://docs.angularjs.org/api/ngMockE2E.$httpBackend
[ngmock]: http://docs.angularjs.org/api/ngMock.$httpBackend
[plunker-step1]: http://plnkr.co/edit/6LInNX?p=preview
[gist-step1]: https://gist.github.com/mrgamer/7056200


[localstorage]: http://caniuse.com/namevalue-storage
[sqlite]: http://caniuse.com/sql-storage
[indexeddb]: http://caniuse.com/indexeddb

[endlesslink]: http://endlessindirection.wordpress.com/2013/05/18/angularjs-delay-response-from-httpbackend/

[plunker-step2]: http://
