---
title: "Firebase Job Dispatcher"
description: "This guide discusses how to schedule background work using the Firebase Job Dispatcher library from Google."
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
---

# Firebase Job Dispatcher

_This guide discusses how to schedule background work using the Firebase Job Dispatcher library from Google._

## Overview

One of the best ways to keep an Android application responsive to the user is to ensure that complex or long running work is performed in the background. However, it is important that background work will not negatively impact the user's experience with the device. 

For example, a background job might poll a website every three or four minutes to query for changes to a particular dataset. This seems benign, however it would have a disastrous impact on battery life. The application will repeatedly wake up the device, elevate the CPU to a higher power state, power up the radios, make the network requests, and then processing the results. It gets worse because the device will not immediately power down and return to the low-power idle state. Poorly scheduled background work may inadvertently keep the device in a state with unnecessary and excessive power requirements. This seemingly innocent activity (polling a website) will render the device unusable in a relatively short period of time.

Android provides the following APIs to help with performing work in the background but by themselves they are not sufficient for intelligent job scheduling. 

- **[Intent Services](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Intent Services are great for performing the work, however they provide no way to schedule work.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; These APIs only allow work to be scheduled but provide no way to actually perform the work. Also, the AlarmManager only allows time based constraints, which means raise an alarm at a certain time or after a certain period of time has elapsed. 
- **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; The JobSchedule is a great API that works with the operating system to schedule jobs. However, it is only available for those Android apps that target API level 21 or higher. 
- **[Broadcast Receivers](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; An Android app can setup broadcast receivers to perform work in response to system-wide events or Intents. However, broadcast receivers don't provide any control over when  the job should be run. Also changes in the Android operating system will restrict when broadcast receivers will work, or the kinds of work that they can respond to. 

There are two key features to efficiently performing background work (sometimes referred to as a _background job_ or a _job_):

1. **Intelligently scheduling the work** &ndash; It is important that when an application is doing work in the background that it does so as a good citizen. Ideally, the application should not demand that a job be run. Instead, the application should specify conditions that must be met for when the job can run, and then schedule  that work to run when the conditions are met. This allows Android to intelligently perform work. For example, network requests may be batched to run all at the same time to make maximum use of overhead involved with networking.
2. **Encapsulating the work** &ndash; The code to perform the background work should be encapsulated in a discrete component that can be run independently of the user interface and will be relatively easy to reschedule if the work fails to complete for some reason.

The Firebase Job Dispatcher is a library from Google that provides a fluent API to simplify scheduling background work. It is intended to be the replacement for Google Cloud Manager. The Firebase Job Dispatcher consists of the following APIs:

- A `Firebase.JobDispatcher.JobService` is an abstract class that must be extended with the logic that will run in the background job.
- A `Firebase.JobDispatcher.JobTrigger` declares when the job should be started. This is typically expressed as a window of time, for example, wait at least 30 seconds before starting the job, but run the job within 5 minutes.
- A `Firebase.JobDispatcher.RetryStrategy` contains information about what should be done when a job fails to execute properly. The retry strategy specifies how long to wait before trying to run the job again. 
- A `Firebase.JobDispatcher.Constraint` is an optional value that describes a condition that must be met before the job can run, such as the device is on an unmetered network or charging.
- The `Firebase.JobDispatcher.Job` is an API that unifies the previous APIs in to a unit-of-work that can be scheduled by the `JobDispatcher`. The `Job.Builder` class is used to instantiate a `Job`.
- A `Firebase.JobDispatcher.JobDispatcher` uses the previous three APIs to schedule the work with the operating system and to provide a way to cancel jobs, if necessary.

To schedule work with the Firebase Job Dispatcher, a Xamarin.Android application must encapsulate the code in a type that extends the `JobService` class. `JobService` has three lifecycle methods that can be called during the lifetime of the job:

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; This method is where the work will occur and should always be implemented. It runs on the main thread. This method will return `true` if there is work remaining, or `false` if the work is done. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; This is called when the job is stopped for some reason. It should return `true` if the job should be rescheduled for later.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; This method is called when the `JobService` has finished any asynchronous work. 

To schedule a job, the application will instantiate a `JobDispatcher` object. Then, a `Job.Builder` is used to create a `Job` object, which is provided to the `JobDispatcher` which will try and schedule the job to run.

This guide will discuss how to add the Firebase Job Dispatcher to a Xamarin.Android application and use it to schedule background work.

## Requirements

The Firebase Job Dispatcher requires Android API level 9 or higher. The Firebase Job Dispatcher library relies on some components provided by Google Play Services; the device must have Google Play Services installed.

## Using the Firebase Job Dispatcher Library in Xamarin.Android

To get started with the Firebase Job Dispatcher, first add the [Xamarin.Firebase.JobDispatcher NuGet package](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) to the Xamarin.Android project. Search the NuGet Package Manager for the **Xamarin.Firebase.JobDispatcher** package (which is still in pre-release).

After adding the Firebase Job Dispatcher library, create a `JobService` class and then schedule it to run with an instance of the `FirebaseJobDispatcher`.

### Creating a JobService

All work performed by the Firebase Job Dispatcher library must be done in a type that extends the `Firebase.JobDispatcher.JobService` abstract class. Creating a `JobService` is very similar to creating a `Service` with the Android framework: 

1. Extend the `JobService` class
2. Decorate the subclass with the `ServiceAttribute`. Although not strictly required, it is recommended to explicitly set the `Name` parameter to help with debugging the `JobService`. 
3. Add an `IntentFilter` to declare the `JobService` in the **AndroidManifest.xml**. This will also help the Firebase Job Dispatcher library locate and invoke the `JobService`.

The following code is an example of the simplest `JobService` for an application, using the TPL to asynchronously perform some work:

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### Creating a FirebaseJobDispatcher

Before any work can be scheduled, it is necessary to create a `Firebase.JobDispatcher.FirebaseJobDispatcher` object. The `FirebaseJobDispatcher` is responsible for scheduling a `JobService`. The following code snippet is one way to create an instance of the `FirebaseJobDispatcher`: 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

In the previous code snippet, the `GooglePlayDriver` is class that helps the `FirebaseJobDispatcher` interact with some of the scheduling APIs in Google Play Services on the device. The parameter `context` is any Android `Context`, such as an Activity. Currently the `GooglePlayDriver` is the only `IDriver` implementation in the Firebase Job Dispatcher library. 

The Xamarin.Android binding for the Firebase Job Dispatcher provides an extension method to create a `FirebaseJobDispatcher` from the `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Once the `FirebaseJobDispatcher` has been instantiated, it is possible to create a `Job` and run the code in the `JobService` class. The `Job` is created by a `Job.Builder` object and will be discussed in the next section.

### Creating a Firebase.JobDispatcher.Job with the Job.Builder

The `Firebase.JobDispatcher.Job` class is responsible for encapsulating the meta-data necessary to run a `JobService`. A`Job` contains information such as any constraint that must be met before the job can run, if the `Job` is recurring, or any triggers that will cause the job to be run.  As a bare minimum, a `Job` must have a _tag_ (a unique string that identifies the job to the `FirebaseJobDispatcher`) and the type of the `JobService` that should be run. The Firebase Job Dispatcher will instantiate the `JobService` when it is time to run the job.  A `Job` is created by using an instance of the `Firebase.JobDispatcher.Job.JobBuilder` class. 

The following code snippet is the simplest example of how to create a `Job` using the Xamarin.Android binding:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

The `Job.Builder` will perform some basic validation checks on the input values for the job. An exception will be thrown if it not possible for the `Job.Builder` to create a `Job`.  The `Job.Builder` will create a `Job` with the following defaults:

- A `Job`'s _lifetime_ (how long it will be scheduled to run) is only until the device reboots &ndash; once the device reboots the `Job` is lost.
- A `Job` is not recurring &ndash; it will only run once.
- A `Job` will be scheduled to run as soon as possible.
- The default retry strategy for a `Job` is to use an _exponential backoff_ (discussed on more detail below in the section [Setting a RetryStrategy](#Setting_a_RetryStrategy))

### Scheduling a job

After creating the `Job`, it needs to be scheduled with the `FirebaseJobDispatcher` before it is run. There are two methods for scheduling a `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

The value returned by `FirebaseJobDispatcher.Schedule` will be one of the following integer values:

- `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; The `Job` was successfully scheduled.
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Some unknown problem occurred which prevented the `Job` from being scheduled.
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; An invalid `IDriver` was used or the `IDriver` was somehow unavailable. 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; The `Trigger` was not supported.
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; The service is not configured correctly or is unavailable.

### Configuring a job

It is possible to customize a job. Examples of how a job may be customized include the following:

- [Passing Parameters to a Job](#Passing_Parameters_to_a_Job) &ndash; A `Job` may require additional values to perform its work, for example downloading a file.
- [Set Constraints](#Setting_Constraints) &ndash; It may be necessary to only run a job when certain conditions are met. For example, only run a `Job` when the device is charging. 
- [Specify when a `Job` should run](#Setting_Job_Triggers) &ndash; The Firebase Job Dispatcher allows applications to specify a time when the job should run.  
- [Declare a retry strategy for failed jobs](#Setting_a_RetryStrategy) &ndash; A _retry strategy_ provides guidance to the `FirebaseJobDispatcher` on what to do with `Jobs` that fail to complete. 

Each of these topics will be discussed more in the following sections.

<a name="Passing_Parameters_to_a_Job"></a>

#### Passing parameters to a job

Parameters are passed to a job by creating a `Bundle` that is passed along with the `Job.Builder.SetExtras` method:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

The `Bundle` is accessed from the `IJobParameters.Extras` property on the `OnStartJob` method:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints"></a>

#### Setting constraints

Constraints can help reduces costs or battery drain on the device. The `Firebase.JobDispatcher.Constraint` class defines these constraints as integer values:

- `Constraint.OnUnmeteredNetwork` &ndash; Only run the job when the device is connected to an unmetered network. This is useful to prevent the user from incurring data charges.
- `Constraint.OnAnyNetwork` &ndash; Run the job on whatever network the device is connected to. If specified along with `Constraint.OnUnmeteredNetwork`, this value will take priority.
- `Constraint.DeviceCharging` &ndash; Run the job only when the device is charging.

Constraints are set with the `Job.Builder.SetConstraint` method: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers"></a>

The `JobTrigger` provides guidance to the operating system about when the job should start. A `JobTrigger` has an _executing window_ that defines a scheduled time for when the `Job` should run. The execution window has a _start window_ value and an _end window_ value. The start window is the number of seconds that the device should wait before running the job and the end window value is the maximum number of seconds to wait before running the `Job`. 

A `JobTrigger` can be created with the `Firebase.Jobdispatcher.Trigger.ExecutionWindow` method.  For example `Trigger.ExecutionWindow(15,60)` means that the job should run between 15 and 60 seconds from when it is scheduled. The `Job.Builder.SetTrigger` method is used to 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

The default `JobTrigger` for a job is represented by the value `Trigger.Now`, which specifies that a job be run as soon as possible after being scheduled..

<a name="Setting_a_RetryStrategy"></a>

#### Setting a RetryStrategy

The `Firebase.JobDispatcher.RetryStrategy` is used to specify how much of a delay a device should use before trying to re-run a failed job. A `RetryStrategy` has a _policy_, which defines what time-base algorithm will be used to re-schedule the failed job, and an execution window that specifies a window in which the job should be scheduled. This _rescheduling window_ is defined by two values. The first value is the number of seconds to wait before rescheduling the job (the _initial backoff_ value), and the second number is the maximum number of seconds before the job must run (the _maximum backoff_ value). 

The two types of retry policies are identified by these int values:

- `RetryStrategy.RetryPolicyExponential` &ndash; An _exponential backoff_  policy will increase the initial backoff value exponentially after each failure. The first time a job fails, the library will wait the _initial interval that is specified before rescheduling the job &ndash; example 30 seconds. The second time the job fails, the library will wait at least 60 seconds before trying to run the job. After the third failed attempt, the library will wait 120 seconds, and so on. The default `RetryStrategy` for the Firebase Job Dispatcher library is represented by the `RetryStrategy.DefaultExponential` object. It has an initial backoff of 30 seconds and a maximum backoff of 3600 seconds.
- `RetryStrategy.RetryPolicyLinear` &ndash; This strategy is a _linear backoff_  that the job should be rescheduled to run at set intervals (until it succeeds). Linear backoff is best suited for work that must be completed as soon as possible or for problems that will quickly resolve themselves. The Firebase Job Dispatcher library defines a `RetryStrategy.DefaultLinear` which has a rescheduling window of at least 30 seconds and up to 3600 seconds.

It is possible to define a custom `RetryStrategy` with the `FirebaseJobDispatcher.NewRetryStrategy` method. It takes three parameters:

1. `int policy` &ndash; The _policy_ is one of the previous `RetryStrategy` values, `RetryStrategy.RetryPolicyLinear`, or `RetryStrategy.RetryPolicyExponential`.
2. `int initialBackoffSeconds` &ndash; The _initial backoff_ is a delay, in seconds, that is required before trying to run the job again. The default value for this is 30 seconds. 
3. `int maximumBackoffSeconds` &ndash; The _maximum backoff_ value declares the maximum number of seconds to delay before trying to run the job again. The default value is 3600 seconds. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### Cancelling a job

It is possible to cancel all the jobs that have been scheduled, or just a single job using the `FirebaseJobDispatcher.CancelAll()` method or the `FirebaseJobDispatcher.Cancel(string)` method:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Either method will return an integer value:

- `FirebaseJobDispatcher.CancelResultSuccess` &ndash; The job was successfully cancelled.
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; An error prevented the job from being cancelled.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; The `FirebaseJobDispatcher` is unable to cancel the job as there is no valid `IDriver` available.

## Summary

This guide discussed how to use the Firebase Job Dispatcher to intelligently perform work in the background. It discussed how to encapsulate the work to be performed as a `JobService` and how to use the `FirebaseJobDispatcher` to schedule that work, specifying the criteria with a `JobTrigger` and how failures should be handled with a `RetryStrategy`.

## Related Links

- [Xamarin.Firebase.JobDispatcher on NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [firebase-job-dispatcher on GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Intelligent Job-Scheduling](https://developer.android.com/topic/performance/scheduling.html)
- [Android Battery and Memory Optimizations - Google I/O 2016 (video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
