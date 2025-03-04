# Changelog

## Unreleased

* Upgrade to the async/await ecosystem, including `std::future::Future`, v0.3
  of the futures crate, and v0.2 of Tokio. The minimum supported Rust version
  is now Rust 1.39. Special thanks to [@sd2k] and [@dbcfd]. ([#187])

  The main difference is that functions that previously returned
  ```rust
  futures01::Future<Item = T, Error = E>
  ```
  now return:
  ```rust
  std::future::Future<Output = Result<T, E>>
  ```
  In the special case when the error was `()`, the new signature is further
  simplified to:
  ```rust
  std::future::Future<Output = T>
  ```
  Functions that return `future::Stream`s have had the analogous transformation
  applied.

[#187]: https://github.com/fede1024/rust-rdkafka/pull/187

[@sd2k]: https://github.com/sd2k
[@dbcfd]: https://github.com/dbcfd

<a name="0.22.0"></a>
## 0.22.0 (2019-12-01)

* Add a client for Kafka's Admin API, which allows actions like creating and
  deleting Kafka topics and changing configuration parameters. ([#122])
* Fix compliation on ARM, and ensure it stays fixed by adding an ARM builder
  to CI. ([#134], [#162])
* Stop automatically generating librdkafka bindings. Platform-independent
  bindings are now checked in to the repository. ([#163])
* Move zstd compression support behind the `zstd` feature flag. ([#163])
* Remove build-time dependency on bindgen, clang, and libclang. ([#163])
* Support `Consumer::pause` and `Consumer::resume`. ([#167])
* Expose the `message_queue_nonempty` callback, which allows clients to put
  their poll thread to sleep and be woken up when new data arrives. ([#164])
* Implement `IntoOpaque` for `Arc<T>`. ([#171])
* Add `Consumer::seek` method. ([#172])
* Support building with Microsoft Visual C++ (MSVC) on Windows. ([#176])
* Bump librdkafka to v1.2.2. ([#177])
* Run tests against multiple Kafka versions in CI. ([#182])
* Standardize feature names. All feature names now use hyphens instead of
  underscores, as is conventional, though the old names remain for
  backwards compatibility. ([#183])
* Optionalize libz via a new `libz` feature. The new feature is a default
  feature for backwards compatibility. ([#183])
* Better attempt to make build systems agree on what version of a dependency
  to compile and link against, and document this hazard. ([#183])

[#122]: https://github.com/fede1024/rust-rdkafka/pull/122
[#134]: https://github.com/fede1024/rust-rdkafka/pull/134
[#162]: https://github.com/fede1024/rust-rdkafka/pull/162
[#163]: https://github.com/fede1024/rust-rdkafka/pull/163
[#164]: https://github.com/fede1024/rust-rdkafka/pull/164
[#167]: https://github.com/fede1024/rust-rdkafka/pull/167
[#171]: https://github.com/fede1024/rust-rdkafka/pull/171
[#172]: https://github.com/fede1024/rust-rdkafka/pull/172
[#176]: https://github.com/fede1024/rust-rdkafka/pull/176
[#177]: https://github.com/fede1024/rust-rdkafka/pull/177
[#182]: https://github.com/fede1024/rust-rdkafka/pull/182
[#183]: https://github.com/fede1024/rust-rdkafka/pull/183


<a name="0.21.0"></a>
## 0.21.0 (2019-04-24)

* Add librdkafka 1.0 support
* Automatically generate librdkafka bindings
* Use updated tokio version in asynchronous\_processing example


<a name="0.20.0"></a>
## 0.20.0 (2019-02-25)

* Add FreeBSD support
* Add `offsets_for_times` method
* Add `committed_offsets` method

<a name="0.19.0"></a>
## 0.19.0 (2019-02-06)

* Fix ordering of generics in FutureProducer::send

<a name="0.18.1"></a>
## 0.18.1 (2019-02-06)

* Add method for storing multiple offsets

<a name="0.18.0"></a>
## 0.18.0 (2019-01-18)

* Upgrade librdkafka to 0.11.6

<a name="0.17.0"></a>
## 0.17.0 (2018-06-30)

* Add missing documentation warning.
* Add new experimental producer API. Instead of taking key, value and timestamp directly,
  producers now get them in a `ProducerRecord` which allows to specify optional arguments using
  the builder pattern.
* Add message headers support.
* Upgrade tokio-core to tokio in async example, remove futures-cpupool.
* `MessageStream` is now Send and Sync

<a name="0.16.0"></a>
## 0.16.0 (2018-05-20)

* Upgrade librdkafka to 0.11.4

<a name="0.15.0"></a>
## 0.15.0 (2018-03-15)

* Added iterator interface to the `BaseConsumer`.
* Change timeout to more rust-idiomatic `Option<Duration>`.
* Add `external_lz4` feature to use external lz4 library instead of
  the one one built in librdkafka. Disable by default.
* Mark all `from_ptr` methods as unsafe.
* Remove `Timestamp::from_system_time` and implement `From` trait instead.
* Rename `Context` to `ClientContext`.
* Rename `Empty(...)Context` to `Default(...)Context`.
* Use default type parameters for the context of `Client`, producers and consumers
  with `Default(...)Context` set as the default one.
* Increase default buffer size in `StreamConsumer` from 0 to 10 to reduce context switching.

<a name="0.14.1"></a>
## 0.14.1 (2017-12-30)

* Upgrade to librdkafka 0.11.3
* Add `send_copy_result` method to `FutureProducer`

<a name="0.14.0"></a>
## 0.14.0 (2017-11-26)

#### Features
* Make `PollingProducer` methods public
* Rename `PollingProducer` to `ThreadedProducer`

#### Refactoring
* Remove `TopicConfig` since librdkafka supports default topic configuration
  directly in the top level configuration
* Rename `DeliveryContext` into `DeliveryOpaque`
* Add `IntoOpaque` trait to support different opaque types.

#### Bugs
* Fix regression in producer error reporting (#65)

<a name="0.13.0"></a>
## 0.13.0 (2017-10-22)

#### Refactoring
* Split producer.rs into multiple files
* Both producers now return the original message after failure
* BaseConsumer returns an Option\<Result\> instead of Result\<Option\>

#### Features
* Upgrade to librdkafka 0.11.1
* Enable dynamic linking via feature
* Refactor BaseConsumer, which now implements the Consumer trait directly
* A negative timestamp will now automatically be reported as NonAvailable timestamp
* Point rdkafka-sys to latest librdkafka master branch
* Add producer.flush and producer.in\_flight\_count
* Add max block time for FutureProducer

#### Bugs
* Fix memory leak during consumer error reporting
* Fix memory leak during producer error reporting

<a name="0.12.0"></a>
## 0.12.0 (2017-07-25)

#### Features
* Upgrade librdkafka to 0.11.0.
* `FutureProducer::send_copy` will now return a `DeliveryFuture` direcly.
* TPL entries now also export errors.
* `KafkaError` is now Clone and Eq.

#### Bugs
* Fix flaky tests.

<a name="0.11.1"></a>
## 0.11.1 (2017-06-25)

#### Features

* Support direct creation of OwnedMessages.

<a name="0.11.0"></a>
## 0.11.0 (2017-06-20)

#### Features

* The topic partition list object from librdkafka is now completely accessible
  from Rust.
* The test suite will now run both unit tests and integration tests in
  valgrind, and it will also check for memory leaks.
* rdkafka-sys will use the system librdkafka if it's already installed.
* rdkafka-sys will verify that the crate version corresponds to the librdkafka
  version during the build.
* Timestamp is now Copy.
* Message has been renamed to BorrowedMessage. Borrowed messages can be transformed
  into owned messages. Both implement the new Message trait.
* Improved error enumerations.

#### Bugs

* Fix memory access bug in statistics callback.
* Fix memory leak in topic partition list.
* Messages lifetime is now explicit (issue [#48](https://github.com/fede1024/rust-rdkafka/issues/48))

<a name="0.10.0"></a>
## 0.10.0 (2017-05-15)

#### Features

* Consumer commit callback
* Add configurable poll timeout
* Add special error code for message not received within poll time
* Add topic field for messages
* Make the topic partition list optional for consumer commit
* Add `store_offset` to consumer
* Add at-least-once delivery example

<a name="0.9.1"></a>
## 0.9.1 (2017-04-19)

#### Features

* OpenSSL dependency optional
* librdkafka 0.9.5

<a name="0.9.0"></a>
## 0.9.0 (2017-04-16)

#### Bugs

* Fix termination sequence

#### Features

* Integration tests running in docker and valgrind
* Producer topics are not needed anymore

<a name="0.8.1"></a>
## 0.8.1 (2017-03-21)

##### Bugs

* Implement Clone for `BaseProducerTopic`

#### Features

<a name="0.8.0"></a>
## 0.8.0 (2017-03-20)

#### Features

* Add timestamp support
* librdkafka 0.9.4
* Add client statistics callback and parsing

<a name="0.7.0"></a>
## 0.7.0 (2017-02-18)

#### Features

* Asynchronous message processing example based on tokio
* More metadata for consumers
* Watermark API
* First iteration of integration test suite
