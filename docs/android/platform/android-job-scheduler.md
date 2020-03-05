---
title: "Android Job Scheduler"
description: "This guide discusses how to schedule background work using the Android Job Scheduler API."
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
---

# Android Job Scheduler

_This guide discusses how to schedule background work using the Android Job Scheduler API, which is available on Android devices running Android 5.0 (API level 21) and higher._

## Overview 

One of the best ways to keep an Android application responsive to the user is to ensure that complex or long running work is performed in the background. However, it is important that background work will not negatively impact the user's experience with the device. 

For example, a background job might poll a website every three or four minutes to query for changes to a particular dataset. This seems benign, however it would have a disastrous impact on battery life. The application will repeatedly wake up the device, elevate the CPU to a higher power state, power up the radios, make the network requests, and then processing the results. It gets worse because the device will not immediately power down and return to the low-power idle state. Poorly scheduled background work may inadvertently keep the device in a state with unnecessary and excessive power requirements. This seemingly innocent activity (polling a website) will render the device unusable in a relatively short period of time.

Android provides the following APIs to help with performing work in the background but by themselves they are not sufficient for intelligent job scheduling. 

- **[Intent Services](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Intent Services are great for performing the work, however they provide no way to schedule work.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; These APIs only allow work to be scheduled but provide no way to actually perform the work. Also, the AlarmManager only allows time based constraints, which means raise an alarm at a certain time or after a certain period of time has elapsed. 
- **[Broadcast Receivers](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; An Android app can setup broadcast receivers to perform work in response to system-wide events or Intents. However, broadcast receivers don't provide any control over when the job should be run. Also changes in the Android operating system will restrict when broadcast receivers will work, or the kinds of work that they can respond to. 

There are two key features to efficiently performing background work (sometimes referred to as a _background job_ or a _job_):

1. **Intelligently scheduling the work** &ndash; It is important that when an application is doing work in the background that it does so as a good citizen. Ideally, the application should not demand that a job be run. Instead, the application should specify conditions that must be met for when the job can run, and then schedule that job with the operating system that will perform the work when the conditions are met. This allows Android to run the job to ensure maximum efficiency on the device. For example, network requests may be batched to run all at the same time to make maximum use of overhead involved with networking.
2. **Encapsulating the work** &ndash; The code to perform the background work should be encapsulated in a discrete component that can be run independently of the user interface and will be relatively easy to reschedule if the work fails to complete for some reason.

The Android Job Scheduler is a framework built in to the Android operating system that provides a fluent API to simplify scheduling background work.  The Android Job Scheduler consists of the following types:

- The `Android.App.Job.JobScheduler` is a system service that is used to schedule, execute, and if necessary cancel, jobs on behalf of an Android application.
- An `Android.App.Job.JobService` is an abstract class that must be extended with the logic that will run the job on the main thread of the application. This means that the `JobService` is responsible for how the work is to be performed asynchronously.
- An `Android.App.Job.JobInfo` object holds the criteria to guide Android when the job should run.

To schedule work with the Android Job Scheduler, a Xamarin.Android application must encapsulate the code in a class that extends the `JobService` class. `JobService` has three lifecycle methods that can be called during the lifetime of the job:

- **bool OnStartJob(JobParameters parameters)** &ndash; This method is called by the `JobScheduler` to perform work, and runs on the main thread of the application. It is the responsibility of the `JobService` to asynchronously perform the work and return `true` if there is work remaining, or `false` if the work is done.
    
    When the `JobScheduler` calls this method, it will request and retain a wakelock from Android for the duration of the job. When the job is finished, it is the responsibility of the `JobService` to tell the `JobScheduler` of this fact by call the `JobFinished` method (described next).

- **JobFinished(JobParameters parameters, bool needsReschedule)** &ndash; This method must be called by the `JobService` to tell the `JobScheduler` that the work is done. If `JobFinished` is not called, the `JobScheduler` will not remove the wakelock, causing unnecessary battery drain. 

- **bool OnStopJob(JobParameters parameters)** &ndash; This is called when the job is prematurely stopped by Android. It should return `true` if the job should be rescheduled based on the retry criteria (discussed below in more detail).

It is possible to specify _constraints_ or _triggers_ that will control when a job can or should run. For example, it is possible to constrain a job so that it will only run when the device is charging or to start a job when a picture is taken.

This guide will discuss in detail how to implement a `JobService` class and schedule it with the `JobScheduler`.

## Requirements

The Android Job Scheduler requires Android API level 21 (Android 5.0) or higher. 

## Using the Android Job Scheduler

There are three steps for using the Android JobScheduler API:

1. Implement a JobService type to encapsulate the work.
2. Use a `JobInfo.Builder` object to create the `JobInfo` object that will hold the criteria for the `JobScheduler` to run the job. 
3. Schedule the job using `JobScheduler.Schedule`.

### Implement a JobService

All work performed by the Android Job Scheduler library must be done in a type that extends the `Android.App.Job.JobService` abstract class. Creating a `JobService` is very similar to creating a `Service` with the Android framework: 

1. Extend the `JobService` class.
2. Decorate the subclass with the `ServiceAttribute` and set the `Name` parameter to a string that is made up of the package name and the name of the class (see the following example).
3. Set the `Permission` property on the `ServiceAttribute` to the string `android.permission.BIND_JOB_SERVICE`.
4. Override the `OnStartJob` method, adding the code to perform the work. Android will invoke this method on the main thread of the application to run the job. Work that will take longer that a few milliseconds should be performed on a thread to avoid blocking the application.
5. When the work is done, the `JobService` must call the `JobFinished` method. This method is how `JobService` tells the `JobScheduler` that work is done. Failure to call `JobFinished` will result in the `JobService` putting unnecessary demands on the device, shortening the battery life. 
6. It is a good idea to also override the `OnStopJob` method. This method is called by Android when the job is being shut down before it is finished and provides the `JobService` with an opportunity to properly dispose of any resources. This method should return `true` if it is necessary to reschedule the job, or `false` if it is not desirable to re-run the job.

The following code is an example of the simplest `JobService` for an application, using the TPL to asynchronously perform some work:

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### Creating a JobInfo to schedule a job

Xamarin.Android applications do not instantiate a `JobService` directly, instead they will pass a `JobInfo` object to the `JobScheduler`. The `JobScheduler` will instantiate the requested `JobService` object, scheduling and running the `JobService` according to the metadata in the `JobInfo`. A `JobInfo` object must contain the following information:

- **JobId** &ndash; this is an `int` value that is used to identify a job to the `JobScheduler`. Reusing this value will update any existing jobs. The value must be unique for the application. 
- **JobService** &ndash; this parameter is a `ComponentName` that explicitly identifies the type that the  `JobScheduler` should use to run a job. 

This extension method demonstrates how to create a `JobInfo.Builder` with an Android `Context`, such as an Activity:

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob and sets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creates a JobInfo object.
```

A powerful feature of the Android Job Scheduler is the ability to control when a job runs or under what conditions a job may run. The following table describes some of the methods on `JobInfo.Builder` that allow an app to influence when a job can run:  

|  Method | Description   |
|---|---|
| `SetMinimumLatency`  | Specifies that a delay (in milliseconds) that should be observed before a job is run. |
| `SetOverridingDeadline`  | Declares the that the job must run before this time (in milliseconds) has elapsed. |
| `SetRequiredNetworkType`  | Specifies the network requirements for a job. |
| `SetRequiresBatteryNotLow` | The job may only run when the device is not displaying a "low battery" warning to the user. |
| `SetRequiresCharging` | The job may only run when the battery is charging. |
| `SetDeviceIdle` | The job will run when the device is busy. |
| `SetPeriodic` | Specifies that the job should be regularly run. |
| `SetPersisted` | The job should perisist across device reboots. | 

The `SetBackoffCriteria` provides some guidance on how long the `JobScheduler` should wait before trying to run a job again. There are two parts to the backoff criteria: a delay in milliseconds (default value of 30 seconds)and type of back off that should be used (sometimes referred to as the _backoff policy_ or the _retry policy_). The two policies are encapsulated in the `Android.App.Job.BackoffPolicy` enum:

- `BackoffPolicy.Exponential` &ndash; An exponential backoff policy will increase the initial backoff value exponentially after each failure. The first time a job fails, the library will wait the initial interval that is specified before rescheduling the job – example 30 seconds. The second time the job fails, the library will wait at least 60 seconds before trying to run the job. After the third failed attempt, the library will wait 120 seconds, and so on. This is the default value.
- `BackoffPolicy.Linear` &ndash; This strategy is a linear backoff that the job should be rescheduled to run at set intervals (until it succeeds). Linear backoff is best suited for work that must be completed as soon as possible or for problems that will quickly resolve themselves. 

For more details on create a `JobInfo` object, please read [Google's documentation for the `JobInfo.Builder` class](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### Passing parameters to a job via the JobInfo

Parameters are passed to a job by creating a `PersistableBundle` that is passed along with the `Job.Builder.SetExtras` method:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

The `PersistableBundle` is accessed from the `Android.App.Job.JobParameters.Extras` property in the `OnStartJob` method of a `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### Scheduling a job

To schedule a job, a Xamarin.Android application will get a reference to the `JobScheduler` system service and call the `JobScheduler.Schedule` method with the `JobInfo` object that was created in the previous step. `JobScheduler.Schedule` will immediately return with one of two integer values:

- **JobScheduler.ResultSuccess** &ndash; The job has been successfully scheduled. 
- **JobScheduler.ResultFailure** &ndash; The job could not be scheduled. This is typically caused by conflicting `JobInfo` parameters.

This code is an example of scheduling a job and notifying the user of the results of the scheduling attempt:

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
    snackBar.Show();
}
else
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
    snackBar.Show();
}
```

### Cancelling a job

It is possible to cancel all the jobs that have been scheduled, or just a single job using the `JobsScheduler.CancelAll()` method  or the `JobScheduler.Cancel(jobId)` method:

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## Summary

This guide discussed how to use the Android Job Scheduler to intelligently perform work in the background. It discussed how to encapsulate the work to be performed as a `JobService` and how to use the `JobScheduler` to schedule that work, specifying the criteria with a `JobTrigger` and how failures should be handled with a `RetryStrategy`.

## Related Links

- [Intelligent Job-Scheduling](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API reference](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Scheduling jobs like a pro with JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android Battery and Memory Optimizations - Google I/O 2016 (video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
