# TAP Buildpacks Workshop

## Pre-requisites
* [Kubectl CLI](https://kubernetes.io/docs/tasks/tools/)
* [Tanzu CLI](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.6/tap/install-tanzu-cli.html#install-the-tanzu-cli-4)
* [Tanzu CLI Plugins](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.6/tap/install-tanzu-cli.html#install-tanzu-cli-plugins-5)
* [kp CLI](https://github.com/buildpacks-community/kpack-cli) (optional)
* [k9s](https://k9scli.io/topics/install/) (optional)
* Clone this repo locally. Most of the commands will be executed from this folder and spring-petclinic workload folder.

## Agenda
* Introduction & Setup (15 minutes)
* Why Cloud Native Buildpacks, and what problems are we trying to solve? (10 minutes)

## Introduction to Tanzu Build Service (20 minutes)
  * Technical Architecture
  * OSS and TBS Differences
  * Standard build system across On-Prem, Cloud & Azure Spring Apps  Enterprise

## [Demo & Walkthrough - Image Build](session-1.md) (55 minutes)
  * Download the Project from the Tanzu Developer Portal
  * Initiate application build with buildpacks & kaniko
  * View Image CR
  * Validate created image is added in the Container Repository
  * View image builds
  * View build logs
  * View/Download SBOM through TDP/ Tanzu Insights CLI

## [Demo & Walkthrough - Build Customizations](session-2.md) (55 minutes)
  * Use a different JRE than BellSoft Liberica 
    * Create Azure Java Buildpack
    * Create new stack to use tiny stack
    * Create new builder to use Azure Java CNB
  * Bindings - Bind Azure Application Insights APM with workload

## [Demo & Walkthrough - Patch/Updates at scale](session-3.md) (55 minutes)
  * Update Buildpack Version
  * Update Stack

## Feedback & Open Discussion (25 minutes)

>Total Workshop Duration - 4 Hours**
