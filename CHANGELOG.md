# Change Log
All notable changes to this project will be documented in this file.

## 2.3.0-beta (2018-08-28)
### AWSXRayRecorder.Core (2.3.0-beta)
#### Breaking Change - .NET and .NET Core
- The default sampling strategy now uses [sampling rules](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-dotnet-configuration.html#xray-sdk-dotnet-configuration-sampling). The strategy now runs background tasks that poll X-Ray for updated sampling rules and targets. If you currently use a configuration file to define local sampling rules, the SDK will use those rules as a backup to rules that are defined in the console. To use only local rules, build the recorder with a `LocalizedSamplingStrategy`.

- Removed `SampleDecision Sample(string serviceName, string path, string method);` and `SampleDecision Sample(HttpRequestMessage request);` from `ISamplingStrategy` interface.

- Renamed  namespace for`LocalizedSamplingStartegy.cs` from `Amazon.XRay.Recorder.Core.Sampling` to `Amazon.XRay.Recorder.Core.Sampling.Local`

- Changed constant `UdpSegmentEmitter.EnvironmentVariableDaemonAddress` to `DaemonConfig.EnvironmentVariableDaemonAddress` 

- Changed `void BeginSegment(string name, string traceId, string parentId = null, SampleDecision sampleDecision = SampledDecision.Sampled);` to `void BeginSegment(string name, string traceId = null, string parentId = null, SamplingResponse? samplingResponse = null, DateTime? timestamp = null););` of `IAWSXRayRecorder` interface and `AWSXRayRecorder` class.

- The `BeginSegment()` method now uses the recorder's sampling strategy to make a sampling decision if `SampleDecision` is not present in `SamplingResponse` instance as an argument. Previously, the segment would be sampled by default.

- Removed `EndSegment()`, `EndSegment(decimal timestamp)` and added `void EndSegment(DateTime? timestamp = null);` on `IAWSXRayRecorder` interface and `AWSXRayRecorder` class.

- Removed ` public void BeginSegment(string name, string traceId, decimal timestamp, string parentId = null, SampleDecision? sampleDecision = null)` on `AWSXRayRecorder` instance.

- .NET Core : Changed method from `AWSXRayRecorder.InitializeInstance(IConfiguration configuration, AWSXRayRecorder recorder)` to `AWSXRayRecorder.InitializeInstance(IConfiguration configuration = null, AWSXRayRecorder recorder = null)`

#### Added
##### .NET and .NET Core 
- Added `ShoudTrace()` method to `ISamplingStartegy` interface

- Environment variable `AWS_TRACING_DAEMON_ADDRESS` and `WithDaemonAddress()` on `AwsXrayRecorderBuilder.cs` class can now take a value of the form `127.0.0.1:2000` or `tcp:127.0.0.1:2000 udp:127.0.0.2:2001` or `udp:127.0.0.1:2000 tcp:127.0.0.2:2001`. The former one means UDP and TCP are running at the same address and the later ones specify individual addresses for tcp and udp. By default it assumes a X-Ray daemon running at 127.0.0.1:2000 listening to both UDP and TCP traffic.

- Update DefaultSamplingRules.json file. i.e. `service_name` has been replaced to `host` and version changed to `2`. SDK still supports v1 JSON file.
 
- Adding support for reading context missing startegy setting from IConfiguration : PR [#35](https://github.com/aws/aws-xray-sdk-dotnet/pull/35) for .NET Core and from Appsettings.json for .NET

### AWSXRayRecorder.Handlers.System.Net (2.3.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.SqlServer (2.3.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AwsSdk (2.3.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AspNet (2.3.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AspNetCore (2.3.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

## 2.2.1-beta (2018-06-11)
### AWSXRayRecorder.Core (2.2.1-beta)
#### Added
- .NET and .NET Core : Added `WithSegmentEmitter()` on `AwsXrayRecorderBuilder` class to accept custom `ISegmentEmitter` instance
- .NET and .NET Core : `WithSamplingStrategy` throws `ArgumentNullException` on null argument

### AWSXRayRecorder.Handlers.System.Net (2.2.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.SqlServer (2.2.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AwsSdk (2.2.1-beta)
#### Fixed : .NET and .NET Core
- Handling null value for `RegionEndpoint` of AWS SDK client PR: [#22](https://github.com/aws/aws-xray-sdk-dotnet/pull/22)

### AWSXRayRecorder.Handlers.AspNet (2.2.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AspNetCore (2.2.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

## 2.2.0-beta (2018-03-20)
### AWSXRayRecorder.Core (2.2.0-beta)
#### Fixed
- .NET and .NET Core : Fixed serialization issue for Http method PR: [#12](https://github.com/aws/aws-xray-sdk-dotnet/pull/12), `Amazon.Util.Internal.ConstantClass` PR: [#16](https://github.com/aws/aws-xray-sdk-dotnet/pull/16)
- .NET and .NET Core : Setting custom recorder to `AWSXRayRecorder.Instance` (issue: [#18](https://github.com/aws/aws-xray-sdk-dotnet/issues/18))

### AWSXRayRecorder.Handlers.System.Net (2.2.0-beta)
#### Added
- .NET and .NET Core: Support for `HttpClient` class (PR: [#5](https://github.com/aws/aws-xray-sdk-dotnet/pull/5))

### AWSXRayRecorder.Handlers.SqlServer (2.2.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AwsSdk (2.2.0-beta)
#### Added : .NET and .NET Core
- Added AWS S3 service in the AWS service manifest file
- Added `select` attribute for Dynamo DB and `InvocationType` attribute for Lambda service

### AWSXRayRecorder.Handlers.AspNet (2.2.0-beta)
#### Fixed
- Null reference exception for empty user agent. Issue [#14](https://github.com/aws/aws-xray-sdk-dotnet/issues/14), PR [#15](https://github.com/aws/aws-xray-sdk-dotnet/pull/15)

### AWSXRayRecorder.Handlers.AspNetCore (2.2.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

## 2.1.0-beta (2018-02-20)
### AWSXRayRecorder.Core (2.1.0-beta)
#### Changed
- .NET and .NET Core : Updated `sdk` attribute of the Trace
- .NET and .NET Core : Changed return type of `TraceMethodAsync()` method to `Task` (issue: [#9](https://github.com/aws/aws-xray-sdk-dotnet/issues/9))

### AWSXRayRecorder.Handlers.System.Net (2.1.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.SqlServer (2.1.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AwsSdk (2.1.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AspNet (2.1.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AspNetCore (2.1.0-beta)
- Bumped version to address AWSXRayRecorder.Core package change

## 2.0.1-beta (2018-02-12)
### AWSXRayRecorder.Core (2.0.1-beta)
#### Fixed
- .NET Core : In AWS Lambda environment, TraceContext.GetEntity() is now casted to Entity instead of Subsegment
- .NET and .NET Core : Improve logging

### AWSXRayRecorder.Handlers.System.Net (2.0.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.SqlServer (2.0.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AwsSdk (2.0.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AspNet (2.0.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

### AWSXRayRecorder.Handlers.AspNetCore (2.0.1-beta)
- Bumped version to address AWSXRayRecorder.Core package change

## 2.0.0-beta (2018-02-05)
### AWSXRayRecorder.Core (2.0.0-beta)
#### Added
- .NET Core 2.0 support
- Added AWS Lambda support for .NET Core 2.0 and above
- Added Elastic Beanstalk and ECS plugins

### AWSXRayRecorder.Handlers.System.Net (2.0.0-beta)
#### Added
- .NET Core 2.0 support
- Added support for asynchronous method calls

### AWSXRayRecorder.Handlers.SqlServer (2.0.0-beta)
#### Added
- .NET Core 2.0 support
- Added support for asynchronous method calls

### AWSXRayRecorder.Handlers.AwsSdk (2.0.0-beta)
#### Added
- .NET Core 2.0 support

#### changed
- AWS SDK Handler is changed. Deprecated - AWSSdkTracingHandler class

### AWSXRayRecorder.Handlers.AspNet (2.0.0-beta)
- Package for ASP.NET Framework

### AWSXRayRecorder.Handlers.AspNetCore (2.0.0-beta)
- Package for ASP.NET Core 2.0 Framework

### Deprecated
### AWSXRayRecorder.Handlers.AspNet.WebApi (1.1.2)
- This functionality is included in AWSXRayRecorder.Handlers.AspNet package

## 1.1.2 (2017-10-17)
### AWSXRayRecorder.Core (1.1.2)
#### Changed
- Reorganize 'aws.xray' filed to move runtime related informantion into 'service' field.

### AWSXRayRecorder.Handlers.System.Net (1.1.2)
- Just bump up version for GA

### AWSXRayRecorder.Handlers.AspNet.WebApi (1.1.2)
- Just bump up version for GA

### AWSXRayRecorder.Handlers.SqlServer (1.1.2)
- Just bump up version for GA

### AWSXRayRecorder.Handlers.AwsSdk (1.1.2)
- Just bump up version for GA

#### Fixed
- Exceptions that Entity and Subsegment are not decorated as Serializable.

## 1.1.1
### AWSXRayRecorder.Core (1.1.1)
#### Changed
- Reorganize 'aws.xray' filed to move runtime related informantion into 'service' field.

#### Fixed
- Exceptions that Entity and Subsegment are not decorated as Serializable.

## 1.1.0 (2017-04-18)
### AWSXRayRecorder.Core (1.1.0)
- Just bump up version for GA

### AWSXRayRecorder.Handlers.System.Net (1.1.0)
- Just bump up version for GA

### AWSXRayRecorder.Handlers.AspNet.WebApi (1.1.0)
- Just bump up version for GA

### AWSXRayRecorder.Handlers.SqlServer (1.1.0)
- Just bump up version for GA

### AWSXRayRecorder.Handlers.AwsSdk (1.1.0)
- Just bump up version for GA

#### Fixed
- Subsegment name for S3 client is not captured correctly.

## 1.0.6-beta (2017-04-12)
### AWSXRayRecorder.Handlers.AwsSdk (1.0.2-beta)
#### Added
- Dynamo ListTables response descriptor to get number of tables returned. 

#### Fixed
- Update usage of interface in sync with Core Runtime 1.0.2-beta

### AWSXRayRecorder.Handlers.System.Net (1.0.2-beta)
#### Fixed
- Update usage of interface in sync with Core Runtime 1.0.2-beta

## 1.0.5-beta (2017-03-30)
### AWSXRayRecorder.Core (1.0.2-beta)
#### Added
- Runtime information to the `aws.xray` namespace on segments.
- Added a `ContextMissingStrategy` property to the `IAWSXRayRecorder` interface. This allows configuration of the exception behavior exhibited when trace context is not properly propagated. The behavior can be configured in code. Alternatively, the environment variable `AWS_XRAY_CONTEXT_MISSING` can be used (overrides any modes set in code). Valid values for this environment variable are currently (case insensitive) `RUNTIME_ERROR` and `LOG_ERROR`. By default, an exception will be thrown on missing context.

#### Changed
- Renamed `SegmentNotAvailableException` to `EntityNotAvailableException`

### AWSXRayRecorder.Handlers.AspNet.WebApi (1.0.1-beta)
#### Added
- Capturing 'x_forwarded_for' header from incoming HTTP request

## 1.0.4-beta (2017-03-06)
### AWSXRayRecorder.Core (1.0.1-beta)
#### Added
- Method `AddMetadata` to `IAWSXRayRecorder` interface.
- Method `SetDaemonAddress` to `IAWSXRayRecorder` interface.
- Propert `RuntimeContext` to `IAWSXRayRecorder` interface.
- `DefaultSamplingRule.json` as embedded resource.
- Requirement to provide default sampling rule, otherwise `InvalidSamplingConfigurationException` will be thrown.
- Constructor to `LocalizedSamplingStrategy` without path which loads default sampling rules.
- Method `Sample(string serviceName, string path, string method)` to interface `ISamplingStrategy`.
- New Exception type `InvalidSamplingConfigurationException`.

#### Changed
- Attribute *aws.xray.sdk* get renamed to *aws.xray.sdk_version*.
- Resolution of start/end time to microsecond.
- Parameter type of method `TryGetRuntimeContext` in intercae `IPlug` from `Dictionary` to `IDictionary`.
- The json format of sampling configuration.

### AWSXRayRecorder.Handlers.AwsSdk (1.0.1-beta)
#### Added
- `DefaultAWSWhitelist.json` as embedded resource.
- Constructor to `AWSSdkTracingHandler` without path which loads default AWS whitelist configuration.

#### Changed
- Rename `TracingEventHandler` to `AWSSdkTracingHandler`.
- Reorganized classed to `Entities` namespace.

### AWSXRayRecorder.Handlers.System.Net (1.0.1-beta)
#### Added
- Feature of setting error/fault/throttle to subsegment based on the response code received from downstream HTTP service.

## 1.0.2-beta (2017-01-25)
### Added
- Package **AWSXRayRecorder.Core**. It provides the core functions to control tracing segment and communication to daemon.
- Package **AWSXRayRecorder.Handlers.AspNet.WebApi**` package. It provides functionality to trace ASP.NET Web API requests.
- Package **AWSXRayRecorder.Handlers.AwsSdk** package. It provides functionality to trace AWSSDK request.
- Package **AWSXRayRecorder.Handlers.SqlServer** package. It provides functionality to trace queries to SQl Server.
- Package **AWSXRayRecorder.Handlers.System.Net**` package. It provides functionality to trace WebRequest from System.Net namespace.
- Environment variable **AWS_XRAY_DAEMON_ADDRESS** to configure target daemon address and port.
- Environment variable **AWS_XRAY_TRACING_NAME** to configure segment name.
- Support of chaining exceptions, which increase efficiency to serialize Exceptions.
- `FixedSegmentNamingStrategy` and `DynamicSegmentNamingStrategy`.
- Attribute *aws.xray.sdk* to capture version of the SDK.

### Deprecated
- **AWSXRayRecorder** package. It has been split into **AWSXRayRecorder.Core** and **AWSXRayRecorder.Handlers** packages. **AWSXRayRecorder** package become a meta-package, which do not contain any code, but only defines dependencies to all subpackages.

### Changed
- Modified `TracingMessageHandler` to require an instance of `SegmentNamingStrategy` when instantiating. A shorthand constructor which accepts a single `string` is also provided to simplify use of the `FixedSegmentNamingStrategy`.

### Fixed
- A bug in the wildcard matching logic used in sampling rules and `TracingMessageHandler`.

### Removed
- Method `AddEventHandler` from interface `IAWSXRayRecorder`

## 1.0.1-beta (2016-12-01)
### Fixed
- Fixed a bug where file handler is not properly closed.