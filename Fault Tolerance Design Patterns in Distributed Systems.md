## Fault Tolerance Design Patterns in Distributed Systems
Distributed systems are made up of multiple interconnected components that work together to provide a service. These components are often geographically dispersed and run on different hardware and software platforms. This complexity makes distributed systems more susceptible to faults and failures than centralised systems.

In distributed systems, a single fault or failure in one component can cause a ripple effect that affects other components and ultimately leads to a system-wide failure. Therefore, fault tolerance is critical in distributed systems to ensure that the system continues to function even in the presence of faults.

A fault-tolerant distributed system is designed to detect, isolate, and recover from faults and failures. It should be able to identify the location and scope of the fault, isolate the affected components, and continue to provide the service with minimal disruption to the end-users.

Without fault tolerance, distributed systems are prone to downtime, data loss, and performance degradation, which can lead to financial losses, reputational damage, and loss of customer trust. Therefore, fault tolerance is a key requirement for any distributed system that aims to provide a reliable and high-performing service.

A fault-tolerant design aims to minimise the impact of faults by anticipating them and designing the system in a way that it can either continue to function or recover from the fault without compromising the overall system performance or reliability.

Let us look at some examples of fault tolerance design patterns. I have chosen Circuit Breaker Design Pattern and Bulkhead Design Pattern

## Circuit Breaker Pattern:
The circuit breaker pattern is a software design pattern that is used to prevent cascading failures in a distributed system. It is named after the circuit breaker in an electrical circuit, which is designed to prevent an electrical overload from causing damage to the system.

![image](https://user-images.githubusercontent.com/7569031/227788541-b0e2ac4c-5ccf-4526-957e-1ac397b884c4.png)

In the context of software architecture, the circuit breaker pattern involves wrapping calls to a remote service or API in a circuit breaker object. This object monitors the number of failures that occur when calling the remote service, and if the number of failures exceeds a certain threshold, the circuit breaker trips and subsequent calls to the remote service are redirected to a fallback method or cached result instead.

This pattern is particularly useful in distributed systems where failures can be caused by network latency, timeouts, or other transient faults. By using a circuit breaker, the system can continue to function even if a particular service or API is experiencing temporary issues. Additionally, the circuit breaker can help to reduce the load on a failing service by redirecting traffic to other healthy services.

Here is a basic structure of the circuit breaker pattern:

1. First, the circuit breaker starts in a “closed” state, allowing requests to pass through to the underlying system.
2. As requests are made, the circuit breaker monitors their success or failure rates. If the failure rate exceeds a certain threshold, the circuit breaker “trips” and moves to an “open” state.
3. While in the “open” state, requests are not allowed to pass through to the underlying system. Instead, the circuit breaker returns an error message to the client.
4. After a set period of time, the circuit breaker “resets” itself and moves back to the “closed” state, allowing requests to pass through again.
5. If the failure rate continues to exceed the threshold while in the “open” state, the circuit breaker may move to a “half-open” state, allowing a limited number of requests to pass through to test if the underlying system is now functioning correctly.
6. If the test requests succeed, the circuit breaker moves back to the “closed” state. If they fail, it moves back to the “open” state.

![image](https://user-images.githubusercontent.com/7569031/227788604-d3c8e520-00fa-459e-91a6-6d126428db95.png)
> State Transition between open and closed state in a circuit breaker pattern

Here are some examples of how the circuit breaker design pattern can be implemented in Python and Ruby:

#### Python
```Python
import requests
import time
class CircuitBreaker:
    def __init__(self, max_failures, reset_timeout):
        self.max_failures = max_failures
        self.reset_timeout = reset_timeout
        self.failure_count = 0
        self.last_failure_time = 0
        self.state = 'closed'
    def call(self, url):
        if self.state == 'open' and time.time() - self.last_failure_time > self.reset_timeout:
            self.state = 'closed'
            self.failure_count = 0
        if self.state == 'closed':
            try:
                response = requests.get(url)
                if response.status_code >= 500:
                    self.failure_count += 1
                    if self.failure_count >= self.max_failures:
                        self.state = 'open'
                        self.last_failure_time = time.time()
                return response
            except Exception as e:
                self.failure_count += 1
                if self.failure_count >= self.max_failures:
                    self.state = 'open'
                    self.last_failure_time = time.time()
                raise e
        else:
            raise Exception('Circuit is open')
breaker = CircuitBreaker(3, 60)
try:
    response = breaker.call('https://example.com')
    print(response.status_code)
except Exception as e:
    print(str(e))
try:
    response = breaker.call('https://example.com')
    print(response.status_code)
except Exception as e:
    print(str(e))
try:
    response = breaker.call('https://example.com')
    print(response.status_code)
except Exception as e:
    print(str(e))
time.sleep(61)
try:
    response = breaker.call('https://example.com')
    print(response.status_code)
except Exception as e:
    print(str(e))
```
    
In this example, the CircuitBreaker class implements the circuit breaker design pattern by tracking the number of failures and the time of the last failure. If the number of failures exceeds a certain threshold, the circuit is tripped and subsequent calls to the service will raise an exception until the circuit is reset. The call() method takes a URL as an argument and attempts to make a request to that URL. If the circuit is open, the method raises an exception without making the request. If the circuit is closed, the method makes the request and checks the response code. If the response code is in the 500 range, the failure count is incremented, and the circuit is tripped if the failure count exceeds the maximum number of failures.

The example shows how the circuit breaker can be used to handle requests to a service that may be experiencing intermittent failures. The try/except blocks demonstrate how the call() method can be used to make requests to the service and handle any exceptions that may be raised. The time.sleep() call is included to demonstrate how the circuit can be reset after a certain amount of time has elapsed since the last failure.

#### Ruby
```Ruby
require 'net/http'
class CircuitBreaker
  def initialize(max_failures, reset_timeout)
    @max_failures = max_failures
    @reset_timeout = reset_timeout
    @failure_count = 0
    @last_failure_time = 0
    @state = 'closed'
  end
  def call(url)
    if @state == 'open' && (Time.now.to_i - @last_failure_time) > @reset_timeout
      @state = 'closed'
      @failure_count = 0
    end
    if @state == 'closed'
      begin
        response = Net::HTTP.get_response(URI(url))
        if response.code.to_i >= 500
          @failure_count += 1
          if @failure_count >= @max_failures
            @state = 'open'
            @last_failure_time = Time.now.to_i
          end
        end
        return response
      rescue => e
        @failure_count += 1
        if @failure_count >= @max_failures
          @state = 'open'
          @last_failure_time = Time.now.to_i
        end
        raise e
      end
    else
      raise 'Circuit is open'
    end
  end
end
breaker = CircuitBreaker.new(3, 60)
begin
  response = breaker.call('https://example.com')
  puts response.code
rescue => e
  puts e.message
end
begin
  response = breaker.call('https://example.com')
  puts response.code
rescue => e
  puts e.message
end
begin
  response = breaker.call('https://example.com')
  puts response.code
rescue => e
  puts e.message
end
sleep(61)
begin
  response = breaker.call('https://example.com')
  puts response.code
rescue => e
  puts e.message
end
```

## Bulkhead Design Pattern
Bulkhead design pattern is a software design pattern that is used to limit the impact of a failure in one part of a system on other parts of the system. This pattern is named after the bulkheads used in ships that divide the ship into watertight compartments, so that if one compartment is breached, the others remain intact.

![image](https://user-images.githubusercontent.com/7569031/227788993-91e3193b-2a5d-450b-8fcc-4aa2e7f4f30d.png)
> A Ship split into multiple heads so that the damage is isolated on a hit

In the context of software architecture, bulkhead pattern involves dividing an application into multiple partitions or compartments, where each partition contains a subset of the system’s functionality. Each partition is designed to be independent of the others, with its own set of resources and threads, so that if one partition fails, it does not affect the other partitions.

![image](https://user-images.githubusercontent.com/7569031/227789109-2c8c7575-b91a-4687-967c-725597e94183.png)
> Bulk Head Design Pattern

This pattern is particularly useful in large-scale distributed systems where different parts of the system may be prone to failure, as it allows failures to be contained within a single partition rather than propagating throughout the entire system.

Here are some examples of how the bulkhead design pattern can be implemented in Python and Ruby:

#### Python
```Python
import concurrent.futures

class BulkheadExecutor:
    def __init__(self, max_threads_per_partition, max_partitions):
        self.max_threads_per_partition = max_threads_per_partition
        self.max_partitions = max_partitions
        self.partitions = [[] for i in range(max_partitions)]
        self.executor = concurrent.futures.ThreadPoolExecutor(
            max_workers=max_threads_per_partition * max_partitions
        )

    def submit(self, task, partition_id):
        if partition_id >= self.max_partitions:
            raise ValueError('Invalid partition ID')
        partition = self.partitions[partition_id]
        if len(partition) >= self.max_threads_per_partition:
            raise ValueError('Partition is full')
        future = self.executor.submit(task)
        partition.append(future)
        return future

    def wait(self, partition_id):
        if partition_id >= self.max_partitions:
            raise ValueError('Invalid partition ID')
        partition = self.partitions[partition_id]
        for future in partition:
            future.result()

    def shutdown(self):
        self.executor.shutdown()

def example_task():
    print('Starting task...')
    import time
    time.sleep(1)
    print('Task complete.')

executor = BulkheadExecutor(2, 3)

executor.submit(example_task, 0)
executor.submit(example_task, 1)
executor.submit(example_task, 2)

executor.wait(0)
executor.wait(1)
executor.wait(2)

executor.shutdown()
```
In this example, the BulkheadExecutor class implements the bulkhead design pattern by using the concurrent.futures module to create a thread pool with a fixed number of threads. The thread pool is divided into multiple partitions, with each partition having a maximum number of threads. Tasks are submitted to a specific partition, and each partition has its own list of futures for the submitted tasks. The wait() method waits for all the futures in a specific partition to complete before returning. The shutdown() method shuts down the thread pool when it is no longer needed.

#### Ruby
```Ruby
require 'concurrent'

class BulkheadExecutor
  def initialize(max_threads_per_partition, max_partitions)
    @max_threads_per_partition = max_threads_per_partition
    @max_partitions = max_partitions
    @partitions = Array.new(max_partitions) { Concurrent::Array.new }
    @executor = Concurrent::ThreadPoolExecutor.new(
      min_threads: max_threads_per_partition * max_partitions,
      max_threads: max_threads_per_partition * max_partitions,
      max_queue: 0,
      fallback_policy: :caller_runs
    )
  end

  def submit(task, partition_id)
    raise ArgumentError.new('Invalid partition ID') if partition_id >= @max_partitions
    partition = @partitions[partition_id]
    raise ArgumentError.new('Partition is full') if partition.size >= @max_threads_per_partition
    future = @executor.post { task.call }
    partition << future
    future
  end

  def wait(partition_id)
    raise ArgumentError.new('Invalid partition ID') if partition_id >= @max_partitions
    partition = @partitions[partition_id]
  end

  def shutdown
    @executor.shutdown
  end
end

example_task = lambda do
  puts 'Starting task...'
  sleep 1
  puts 'Task complete.'
end

executor = BulkheadExecutor.new(2, 3)

executor.submit(example_task, 0)
executor.submit(example_task, 1)
executor.submit(example_task, 2)

executor.wait(0)
executor.wait(1)
executor.wait(2)

executor.shutdown
```
Hope the above explanations and implementation examples gave you a clarity on the fault tolerant design patterns.

Imported from my article [Medium: Fault Tolerance Design Patterns in Distributed Systems](https://medium.com/design-bootcamp/fault-tolerance-design-patterns-in-distributed-systems-49853ad237b4)

