# Additional JUnit5 Setup

By default, JUnit tests are run sequentially in a single thread. To speed up tests, parallel execution can be enabled.&#x20;

Create a `junit-platform.properties` file containing:&#x20;

```properties
junit.jupiter.execution.parallel.enabled=true
junit.jupiter.execution.parallel.mode.default=concurrent
junit.jupiter.execution.parallel.config.strategy=dynamic
junit.jupiter.execution.parallel.config.dynamic.factor=0.95
```

Place this file in the `resources` directory of the test source set, e.g.:

```
src/test/resources/junit-platform.properties

or

src/konsistTest/resources/junit-platform.properties
```

Read more in the official [JUnit5 documentation](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parallel-execution).
