
<img align="right" width="250" src="images/combined_logos.png">

## Background

Achieving determinism on real software systems remains difficult. Even a batch-processing job, whose task is to map input bits to output bits, risks nondeterminism from thread scheduling, system calls, CPU instructions, and leakage of environmental information such as date or CPU model.

## Research Project

In 2015 we asked ourselves why there was no available, deployable way to run real Linux software deterministically. We thus set out to create user-space deterministic execution sandboxes.  This work followed on from our prior work on deterministic parallel programming languages and libraries and deterministic operating systems.

### Detflow

Our first prototype, called DetFlow was described in a paper at
OOPSLA'17 [[1]](#references).  DetFlow uses a mix of language-support and
runtime sandboxing to achieve an end-to-end determinism guarantee.

 * DetFlow is [available on GitHub](https://github.com/iu-parfunc/detflow/).
 * The OOPSLA paper's artifact is available [from the ACM DL](https://dl.acm.org/citation.cfm?doid=3152284.3133897), under "Source Materials","Auxiliary Archive". It is a 5GB download.

In DetFlow, process-parallel programs are allowed if they are controlled by a *parallel coordinator* process written with special language support (a Haskell monad).  Subprocess are determinized using a runtime sandbox, but are sequentialized to keep overheads low.  DetFlow has well below 5\% overhead in all our experiments. However, this prototype uses LD_PRELOAD libc interception which is not sufficiently general or robust for a production quality implementation.

### DetTrace

Our second prototype of a runtime determinism sandbox is called, *DetTrace*.  DetTrace uses ptrace and is much more general than DetFlow, but is also higher-overhead.  In case studies, we have run the build and test code for 12,130 Debian packages inside DetTrace, have run large applications like blender and pdflatex inside the deterministic sandbox.

A preprint for DetTrace will be posted here soon.

## Commercialization

The detbox approach is being commercialized by [Cloudseal Inc](https://cloudseal.io).  Cloudseal is building low-overhead record-and-replay-as-a-service, for bug and crash reproduction. The core of the approach is a deterministic execution capability that minimizes the amount of recording needed, and eliminates unnecessary nondeterminism, which leads to things like flaky tests. (In the production implementation of the approach, binary instrumentation is used to avoid ptrace and achieve both the low overhead of DetFlow and the generality of DetTrace.)

### Related and Prior Work

**Binary Instrumentation**:


[[2]](#references)
[[3]](#references)

**Deterministic Libraries and Languages**:

**Deterministic Operating Systems**:


### References

<a name="oopsla"></a>
 1. (**OOPSLA'17**) ["Monadic Composition for Deterministic, Parallel Batch Processing"](https://2017.splashcon.org/event/splash-2017-oopsla-detflow-a-monad-for-deterministic-parallel-shell-scripting), R Scott, O Navarro Leija, J Devietti, and R Newton, ACM SIGPLAN conference on Object-oriented Programming, Systems, Languages and Applications.

 2. (**PLDI'16**) ["Living on the edge: Rapid-toggling probes with cross modification on x86"](https://dl.acm.org/citation.cfm?id=3062344), B Chamith, B Svensson, L Dalessandro, R Newton. ACM SIGPLAN conference on Programming Language Design and Implementation.

 3. (**PLDI'17**) ["Instruction Punning: Lightweight Instrumentation for x86-64"](https://dl.acm.org/citation.cfm?id=2908084), Buddhika Chamith, Bo Joel Svensson, Luke Dalessandro, Ryan Newton. ACM SIGPLAN conference on Programming Language Design and Implementation.

