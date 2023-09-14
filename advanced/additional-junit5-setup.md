# Additional JUnit5 Setup

By default, JUnit tests are run sequentially in a single thread. To speed up tests parallel execution can be enabled.&#x20;

Create `junit-platform.properties` a file containing:&#x20;

```properties
junit.jupiter.execution.parallel.enabled=true
junit.jupiter.execution.parallel.mode.default=concurrent
junit.jupiter.execution.parallel.config.strategy=dynamic
junit.jupiter.execution.parallel.config.dynamic.factor=0.95
```

Place this file in the `resources`  directory of the test source set e.g:

```
src/test/resource/junit-platform.properties

or

src/konsistTest/resource/junit-platform.properties
```

Read more in the official [JUnit5 documentation](https://junit.org/junit5/docs/5.3.0-M1/user-guide/index.html#writing-tests-parallel-execution).
