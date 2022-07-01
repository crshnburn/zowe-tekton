# Zowe Tekton Plug-in

This Task can be used to run a Zowe CLI command.

## Install the Task

```bash
kubectl apply -f https://raw.githubusercontent.com/crshnburn/zowe-tekton/main/zowe.yaml
```

## Parameters

- **command**: The Zowe CLI command to run as part of the Task (_default_: `-h`)

## Workspaces

- **source**: `PersistentVolumeClaim`-type so that volume can be shared amongst other tasks.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: gradle-source-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

- **zosmf-profiles**: `Secret`-type containing the zosmf profiles that can be used to run the command.

## Platforms

The Task can be run on `linux/amd64` platforms.

## Usage

```yaml
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  name: listjobs
spec:
  params:
    - name: command
      value: jobs ls js
  resources: {}
  taskRef:
    kind: Task
    name: zowecli
  workspaces:
    - name: zosmf-profiles
      secret:
        secretName: zowe-profile
```
