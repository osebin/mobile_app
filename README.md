# MLPerf Mobile App

This project contains the MLPerf mobile app, an app-based implementationn of
[MLPerf Inference](https://github.com/mlperf/inference) tasks.

*Please note that this app is not official yet and the integration with LoadGen
isn't quite complete yet.*

## Overview

The MLPerf app offers a simple mobile UI for executing MLPerf inference tasks
and comparing results. The user can select a task, a supported reference model
(float or quantized), and initiate both latency and accuracy validation for that
task. As single-stream represents the most common inference execution on mobile
devices, that is the default mode of inference measurement, with the results
showing the 90%-ile latency and the task-specific accuracy metric result (e.g.,
top-1 accuracy for image classification).

Several important mobile-specific considerations are addressed in the app:

*   Limited disk space - Certain datasets are quite large (multiple gigabytes),
    which makes an exhaustive evaluation difficult. By default, the app does not
    include the full dataset for validation. The client can optionally push part
    or all of the task validation datasets, depending on their use-case.
*   Device variability - The number of CPU, GPU and DSP/NPU hardware
    permutations in the mobile ecosystem is quite large. To this end, the app
    affords the option to customize hardware execution, e.g., adjusting the
    number of threads for CPU inference, enabling GPU acceleration, or NN API
    acceleration (Android’s ML abstraction layer for accelerating inference).

The initial version of the app builds off of a lightweight, C++ task evaluation
pipeline originally built for
[TensorFlow Lite](https://www.tensorflow.org/lite/). Most of the default MLPerf
inference reference implementations are built in Python, which is generally
incompatible with mobile deployment. This C++ evaluation pipeline has a minimal
set of dependencies for pre-processing datasets and post-processing, is
compatible with iOS and Android (as well as desktop platforms), and integrates
with the standard
[MLPerf LoadGen library](https://github.com/mlperf/inference/tree/master/loadgen).
While the initial version of the app uses TensorFlow Lite as the default
inference engine, the plan is to support addition of alternative inference
frameworks contributed by the broader MLPerf community.

## Requirements

*   [Bazel](https://docs.bazel.build/versions/master/install-ubuntu.html)
*   [Android SDK](https://developer.android.com/studio)
*   Android 6.0+ (Marshmallow+) w/
    [USB debugging enabled](https://developer.android.com/studio/debug/dev-options)

## Getting Started

In order to build the app, first make sure to download the SDK and NDK using the
Android studio. Then set the following environment variables:

```bash
export ANDROID_HOME=Path/to/SDK # Ex: $HOME/Android/Sdk
export ANDROID_NDK_HOME=Path/to/NDK # Ex: $ANDROID_HOME/ndk/(your version)
```

The app can be built with the following command:

```bash
bazel build -c opt --cxxopt='--std=c++14' --config=android_arm64 //java/org/mlperf/inference:mlperf_app
```

Please see [these instructions](prebuilt/README.md) for installing and using the
app.

## FAQ

#### Will this be available in the app store(s)?

Yes, eventually, but not with the 0.5 release.

#### When will an iOS version be avilable?

This is a priority for the community but requires some additional resourcing.

#### Will the app support all MLPerf Inference tasks?

That is the eventual goal. To start, it supports only those tasks specifically
targeting mobile and/or edge use-cases (e.g., Classification w/ MobileNet,
Detection w/ SSD MobileNet).

#### Will the app support more than just TensorFlow Lite for inference?

Yes, that is the plan, though this is largely dependent on contributions from
teams and organizations who desire this.

Please search
https://groups.google.com/forum/#!forum/mlperf-inference-submitters for
additional help and related questions.

#### What is the licience for embedded tflite models?

The licience of those models belongs to the Tensorflow team. Please contact them
for more details.