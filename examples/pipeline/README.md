# Pipeline Experiments

Beaker supports creating pipelines, or chaining tasks together, by configuring
one task to depend on the results of one or more result datasets within the
same experiment.

```yaml
datasets:
- mountPath: ...
  source:
    result: <task-name>
- mountPath: ...
  source:
    result: <another-task-name>
```

The example below demonstrates. The first task `store` creates a result with
a single file. The second task `load` mounts the resulting dataset and prints
its content.

```yaml
version: v2-alpha
tasks:
- name: store
  image:
    docker: busybox:latest
  command: [sh, -c]
  arguments: [echo 'Hello world!' > /output/hello.txt]
  result:
    path: /output
  context:
    cluster: ai2/example

- name: load
  image:
    docker: busybox:latest
  command: [sh, -c]
  arguments: [cat /input/hello.txt]
  datasets:
  - mountPath: /input
    source:
      result: store
  result:
    path: /output # Not used.
  context:
    cluster: ai2/example
```
