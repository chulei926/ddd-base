DDD Base
========
!["Building Status"](https://img.shields.io/github/workflow/status/linux-china/ddd-base/Java%20CI%20with%20Maven?label=Github%20Actions)

Domain Driven Design base package for Java.

# How to use it in Java?

Please refer https://jitpack.io/#linux-china/ddd-base/1.1.1

# Features

* Annotations
* Base classes for entity, domain event etc
* Domain event: follow CloudEvents specification
* DDD Reactive: https://www.reactive-streams.org/

# Components

* Data: Entity, VO and Aggregate
* Behaviour: Repository, Service, Factory and Specification
* Event
* Infrastructure

# Reactive

DDD + Reactive(RSocket) to make context map easy.

# Code Structure

Please visit [src/test/java](https://github.com/linux-china/ddd-base/tree/master/src/test/java/org/mvnsearch/demo/domain) for code structure

If you use Kotlin to develop application, the structure will be different, please add entity, vo and repository in the same kt file.

# Events

Please extend DomainEvent or DomainEventBuilder, then use ApplicationEventPublisher to publish the event.
please refer https://spring.io/blog/2015/02/11/better-application-events-in-spring-framework-4-2

Attention: Spring framework 5.2 will add reactive support:  https://github.com/spring-projects/spring-framework/issues/21831

Event extensions(JavaScript Object):

* Distributed Tracing extension(traceparent, tracestate):  embeds context from Distributed Tracing so that distributed systems can include traces that span an event-driven system.
* Dataref(dataref): reference another location where this information is stored
* Partitioning(partitionkey): This extension defines an attribute for use by message brokers and their clients that support partitioning of events, typically for the purpose of scaling.
* Sequence(sequence): describe the position of an event in the ordered sequence of events produced by a unique event source
* Sampling(sampledrate): Sampling
* Multi-tenant(tenantId): Multi-tenant system support

CloudEvents JSONSchema: https://github.com/cloudevents/spec/blob/v0.3/spec.json

### How to create event class

* Extend CloudEvent class:
```
public class LoginEvent extends CloudEvent<String> {

    public LoginEvent(String email, String ip) {
        setData(email);
        setContentType("text/plain");
        setExtension("ip", ip);
    }
}
```

* Create object directly
```
CloudEvent<String> loginEvent = new CloudEvent<String>("text/plain", "linux_china@hotmail.com");
```

* Event Builder or reactive processor

```
CloudEvent<String> loginEvent = CloudEventBuilder.<String>newInstance().contentType("text/plain").data("linux_china@hotmail.com").build();
```

### ObjectMapper

* ObjectMapper creation
```
ObjectMapper objectMapper = new ObjectMapper();
objectMapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);
objectMapper.enable(SerializationFeature.INDENT_OUTPUT);
```

* write as String
```
objectMapper.writeValueAsString(loginEvent);
```

* read from json text
```
objectMapper.readValue(jsonText, new TypeReference<CloudEvent<String>>() {});
```

### References

* CloudEvents Specification: https://github.com/cloudevents/spec/blob/master/spec.md
* Reactive Streams: http://www.reactive-streams.org/
* DDD Reference: https://domainlanguage.com/wp-content/uploads/2016/05/DDD_Reference_2015-03.pdf
