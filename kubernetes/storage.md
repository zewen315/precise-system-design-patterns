# Storage & Configuration

> Questions to know about persistent data and configuration in Kubernetes.

## Storage

- Why is Pod-local storage ephemeral, and what problem does a PersistentVolume (PV) solve?
- What's the difference between a PersistentVolume and a PersistentVolumeClaim (PV = actual storage resource, PVC = a Pod's request for storage)?
- What is a StorageClass, and how does it enable dynamic provisioning of volumes?
- What access modes exist (`ReadWriteOnce`, `ReadOnlyMany`, `ReadWriteMany`), and why do most cloud block storage volumes only support `ReadWriteOnce`?
- How does a StatefulSet give each replica its own stable PersistentVolume via `volumeClaimTemplates`?

## Configuration

- What's the difference between a ConfigMap and a Secret, and how is a Secret actually protected (or not) by default?
- How does a Pod consume a ConfigMap/Secret (env vars vs mounted volume), and what's the trade-off (env vars don't auto-update; mounted files can)?

## Interview Usage

- Why would you avoid running a stateful database directly on Kubernetes in some designs, and when is it acceptable (operators, cloud-managed volumes)?
- How would you explain where configuration and secrets live for a service in your design?
