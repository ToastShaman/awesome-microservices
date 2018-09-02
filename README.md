# Awesome Microservices

A curated list of additional reading material following my talk at [Zuhlke](https://www.zuehlke.com/gb/en/) Days 2018.

## Recommended Books

* Accelerate: The Science of Lean Software and Devops: Building and Scaling High Performing Technology Organizations by Nicole Forsgren et al: http://amzn.eu/d/8fteU0t
* Building Microservices by Sam Newman: http://amzn.eu/d/5VI7e5F
* Building Microservices **Second edition** by Sam Newman: http://amzn.eu/d/4d7JoEA
* Production-Ready Microservices by Susan Fowler: http://amzn.eu/d/gZPrzBf
* Site Reliability Engineering by Betsy Beyer et al: http://amzn.eu/d/57BjsHi
* REST in Practice: Hypermedia and Systems Architecture by Jim Webber et al: http://amzn.eu/d/1JN6obF
* RESTful Web APIs by Leonard Richardson et al: http://amzn.eu/d/c5RdRAo
* Testing Java Microservices by Alex Soto Bueno et al: http://amzn.eu/d/j9WTt21
* Elastic Leadership by Roy Osherove: http://amzn.eu/d/6GEAft4
* Threat Modeling: Designing for Security by Adam Shostack: http://amzn.eu/d/6S2hIVh
* Cloud Native: Designing change-tolerant software by Cornelia Davis: https://www.manning.com/books/cloud-native

## Recommended Articles

* [Testing Microservices, the sane way](https://medium.com/@copyconstruct/testing-microservices-the-sane-way-9bb31d158c16)
* [Your API versioning is wrong, which is why I decided to do it 3 different wrong ways](https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/)
* [Consumer Driven Contract Testing](https://www.martinfowler.com/articles/consumerDrivenContracts.html)
* [REST without PUT](https://www.thoughtworks.com/radar/techniques/rest-without-put)
* [Trunk Based Development](https://trunkbaseddevelopment.com/)
* [The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
* [ThoughtWorks: Technology Radar](https://www.thoughtworks.com/radar)

## Technologies we used in the Microservices

### Microservice Framework

* [Spring Boot](https://spring.io/projects/spring-boot)
* [RxJava](http://reactivex.io/)
* [Kotlin](https://kotlinlang.org/)
* [Arrow](https://arrow-kt.io/): Functional companion to Kotlin's Standard Library
* [Jackson](https://github.com/FasterXML/jackson): JSON serialization/deserialization

### Core Java Libraries

* [Guava](https://github.com/google/guava)
* [TotallyLazy](https://totallylazy.com/): A complete functional environment for Java
* [jOOL](https://github.com/jOOQ/jOOL): The Missing Parts in Java 8

### Caching

* [Caffeine](https://github.com/ben-manes/caffeine): A high performance caching library for Java 8

### Circuit Breaker

* [Hystrix](https://github.com/Netflix/Hystrix) or [Resilienc4j](https://github.com/resilience4j/resilience4j)

### Monitoring

* [Grafana](https://grafana.com/)
* [Prometheus](https://prometheus.io/)
* [Prometheus push gateway](https://github.com/prometheus/pushgateway) 
* [Grafanalib](https://github.com/weaveworks/grafanalib): Python library for building Grafana dashboards
* [Dropwizard Metrics](https://metrics.dropwizard.io/3.1.0/) or [Micrometer](http://micrometer.io/)

### Networking

* [Jersey HTTP client with reactive extensions](https://jersey.github.io/documentation/latest/client.html)
* [OkHttp](https://square.github.io/okhttp/)
* [JsonPath](https://github.com/json-path/JsonPath): A Java DSL for reading JSON documents.

### XML Parsing

* [XOM](http://www.xom.nu/)

### Testing

* [Rest-assured](http://rest-assured.io/)
* [Wiremock](http://wiremock.org/)
* [AssertJ](https://joel-costigliola.github.io/assertj/): Fluent assertions for Java
* [QuickTheories](https://github.com/ncredinburgh/QuickTheories): Property based testing for Java 8
* [Pitest](http://pitest.org/): Mutation testing for Java
* [Snodge](https://github.com/npryce/snodge): Randomly mutate JSON, XML, HTML forms, text and binary data for fuzz testing

### Performance Testing

* [Vegeta](https://github.com/tsenart/vegeta): HTTP load testing tool and library
* [usl4j](https://codahale.com/usl4j-and-you/): a Java library for modeling system performance given a small set of real-world measurements.

### Security

* [Threat Modelling](https://en.wikipedia.org/wiki/Threat_model)
* [OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_2017_Project)

# Avoid _stringly_ typed stystems

Rather than passing a lot of Strings around in your system, try 
to use strongly typed objects. `ValueTypes` can help with that.

```
import org.jetbrains.annotations.NotNull;

import java.util.Objects;

public abstract class ValueType<T extends Comparable<T>> implements Comparable<T> {

    public final T value;

    public ValueType(T value) {
        this.value = value;
    }

    @Override
    public int compareTo(@NotNull T other) {
        return value.compareTo(other);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        ValueType<?> valueType = (ValueType<?>) o;
        return Objects.equals(value, valueType.value);
    }

    @Override
    public int hashCode() {
        return Objects.hash(value);
    }
}
```

Example usage:

```
public class Username extends ValueType<String> {
    public Username(String value) { super(value); }
}

Username alice = new Username("alice@hotmail.com");
```
