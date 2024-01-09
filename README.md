# tekton-github-actions

- Argo workflows but with catalog of prebuilt tasks: https://github.com/tektoncd/catalog/tree/main/task
- Kind takes up +60 seconds to start
- Tekton is rather a framework and requires dev to handle task status management (exit codes) and correct logging
- Shell scripts based development 
- Local debug with Act
- 3 deployemnt schemes 
  * Individual tasks execution scheduled by github actions 
    * The execution unit is a Task: possibilty to parallelize in multiple VMs
    * Exexution flow in Tekton dashboard can be misleading
    * No task reuse possibility in the steps
    * Ability to keep github actions execution flow (UI) => Task = Step
    * Refactoring (taskName may be hard)
    * Requires to maintain 2 pipelines: github actions pipeline + tekton Pipeline
  * Tekton pipeline: 
    * Execution unit is pipeline. The entire pipeline run on a single gith actions VM
    * No execution flow inside github actions. Single task only
    * Troubleshooting complex pipeline hard without Tekton UI execution flow
    * Task reuse
  * Consider specific projects images build as specific topic and run okdp images with github actions (releases only + act for testing)

# Execution

Example:

There is one Kind cluster per Github actions job:


```shell
$ kind get clusters
  build-cluster-x86-64-base
  build-cluster-x86-64-foundation
```

Two jobs run in parallel:

![Jobs](doc/assets/Jobs.png)

The steps:

![Steps](doc/assets/Steps.png)

