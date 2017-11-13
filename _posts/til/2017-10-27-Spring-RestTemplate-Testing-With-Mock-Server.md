---
layout: post
title:  "Today I Learned: How to use  MockRestServiceServer with custom ClientHttpRequestFactory"
date:   2017-10-27 16:38:42 +0000
categories: spring  resttemplate jaxrsclient junit
---

Despite having some reservations about using Spring RestTemplate for writing JAX-RS client ([see here][comparison]), I chose to use Spring RestTemplate to implement a JAX RS client as I liked that I could write test cases by easily mocking the REST service I was consuming. Spring provides a very nice utility to mock REST service end-point: [MockRestServiceServer]. However, I ran into an pesky issue. 

## Problem

I developed my code along with the test cases using the above nice utility by Spring framework. Next step was to log all incoming and outgoing traffic. I was able to implement the feature by registering a custom [LoggingInterceptor] along with the [BufferingClientHttpRequestFactory]. The logging worked fine but my test cases started to fail. Although I ensured that I use BufferingClientHttpRequestFactory so that the response from the server can be read twice - once for logging and next by the application code, the test code would fail on the second read. 


## Diagnosis

I found that the MockRestServiceServer would override the BufferingClientHttpRequestFactory setup on the RestTemplate instance passed to it while creating the server. 

```java
@Override
public MockRestServiceServer build(RequestExpectationManager manager) {
    MockRestServiceServer server = new MockRestServiceServer(manager);
    MockClientHttpRequestFactory factory = server.new MockClientHttpRequestFactory();
    if (this.restTemplate != null) {
        this.restTemplate.setRequestFactory(factory);
    }
    if (this.asyncRestTemplate != null) {
        this.asyncRestTemplate.setAsyncRequestFactory(factory);
    }
    return server;
}

```

## Solution 

On scouring further, I found that the there is a hack to work around this [issue]. Once the server is created, read the `requestFactory` instance created by the `MockRestServiceServer` using ReflectionTestUtils on RestTemplate instance and wrap it up with BufferingClientHttpRequestFactory.

```java
@Before
public void setUp() throws Exception {
    mockServer = MockRestServiceServer.createServer(restTemplate);

    final ClientHttpRequestFactory requestFactory = 
        (ClientHttpRequestFactory) ReflectionTestUtils.getField(restTemplate, "requestFactory");

    restTemplate.setRequestFactory(new BufferingClientHttpRequestFactory(requestFactory));
}

```

I hope this helps you as well. 

[comparison]:https://dev.to/shriharshmishra/comparing-springs-resttemplate-and-jerseys-client-apis-350
[MockRestServiceServer]:https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/test/web/client/MockRestServiceServer.html
[LoggingInterceptor]:https://stackoverflow.com/a/33009822/7007521 
[BufferingClientHttpRequestFactory]:https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/http/client/BufferingClientHttpRequestFactory.html
[issue]: https://github.com/spring-projects/spring-boot/issues/6686
