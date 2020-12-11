+++
title = "European Environment for Scientific Software Installations (EESSI)"
template = "session.html"
[extra]
authors = "Thomas Röblitz"
session = "3.3"
+++

We will do a presentation of the EESSI (European Environments for Scientific Software Installations) project including a demo of its current pilot software stack.

In a nutshell, EESSI develops an infrastructure/service which will eventually allow you to use the same scientific software stack on any machine (e.g., Raspberry Pi, laptop, server, cluster, cloud, supercomputer) running on various operating systems (Linux, macOS, Windows) and the software stack is built from sources and can thereby be optimised for the CPU/GPU/interconnect at your machine. Even better you don't even have to install (almost) any software package as the stack will be delivered to you via CernVM-FS a proven solution to distribute software in the WLCG (Worldwide LHC Computing Grid).

The current pilot stack can be easily tested via Singularity, supports ARM, Intel and AMD processors and includes scientific software packages such as GROMACS, OpenFOAM, bioconductor, TensorFlow as well as all their dependencies.


---

#### Questions and comments

- Question: Is it possible to test the whole stack, please add links?
    - Yes, see https://eessi.github.io/docs/pilot/
    - To get help, join the EESSI Slack, see https://www.eessi-hpc.org/join/
* Question: Will you also support AMD Rocm and AMD ecosystem overall?
  - Yes, eventually. Right now there already are optimized installations for AMD Zen2 (Rome).
    OpenMPI is included and is installed on top of UCX & libfabric, so *should* properly support AMD Rocm interconnect, but this is currently untested.
* Comment: I like this idea as for us it is important people can use it in their laptops.
  Personally I not much loosing time in setting up sw at my laptop but I see for the users it is important
  to have an option to install/use it also in their lab. They like it more.
  - Yes, this could allow people to *literally* write a job script that just works on the HPC cluster. Same modules, same software.
    (and no need to build containers, or copy them over, etc.)
* Question: This builds on existing projects so it has some content from the begining.
  - Thanks to EasyBuild we can easily provide 1000s of installations.
    Right now we limit what we provide, so we can focus on solving the problems we're hitting first.
* Question: Why European in the name?
  - Because it started with European sites.
    We're already thinking about changing the first E to "Easy" :)
    "EESSI is the Easy Environment..."
* Question: Question: what are the possibilities to add “own dirty module”, is it like same as e.g. with EasyBuild itself?
  - You can easily install additional software on top, for example in your home directory on in /tmp, just like you can with any other software stack built with EasyBuild).
* Question: Sensitivity of central Stratum-0 component, in terms of resilience?
  - The CernVM-FS design is very robust. If the Stratum-0 dies, the only impact is that you can't add new software to the repositories.
    As long as *one* Stratum-1 server is still alive, the software remains available (all Stratum-1 servers have a full copy of the provided software).
    So it comes down to having enough Stratum-1 servers, spread across the world, in different sites and cloud providers.
  - W.r.t adding software: we plan to fully automate the workflow of adding software to the EESSI repository, such that adding software comes down to opening a pull request on GitHub. When the PR is approved by a reviewer, the software gets built automatically on all supported CPU architectures, and added to Stratum-0, fully automatically. Ideally we also have (small) test cases to verify that the installations are functional before deploying them.
* Question: You mentioned that CernVM-FS only relies on HTTP connections. Shouldn't that be HTTPS for security reasons?
  - No, switching to HTTPS has no added value in terms of security, we've discussed that with the CernVM-FS developers.
    CernVM-FS has built in security checks between server and clients, so HTTPS doesn't provide any additional security (I think, should be checked in CernVM-FS documentation).
* How would this work for large jobs across multiple nodes, can a lot of network traffic to pull in the software be avoided?
  - Yes, you can set up a shared CernVM-FS cache on a shared filesystem.
    If there's no internet access on the cluster workernodes, you can use a squid proxy in the cluster network (on a login node for example).
    This setup has been tested with the EESSI pilot stack at the Jülich Supercomputing Centre, worked really well!
* Comment: Detection of CPU architecture is a very nice feature. This is a big issue with containers where generic binaries are often, which can have a big impact on performance.
  - Yes, indeed! Containers are also very rigid: what if you want to add additional software?
    The EESSI environment is way more dynamic, easy to add software on top of it (without paying for it in terms of performance), etc.
* Comment: This would also work really well in heterogenous environments with a mix of old/new CPUs, thanks to the auto-detection mechanism.
  - Yes, very correct, this is an interesting use case!


