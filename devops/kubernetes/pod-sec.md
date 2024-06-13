# `pod-security.kubernetes.io/enforce`

The `pod-security.kubernetes.io/enforce` label is used to specify the Pod Security Standards (PSS) enforcement level for a namespace in Kubernetes. These standards are designed to ensure that pods adhere to certain security practices. There are three main levels of security policies defined by Kubernetes:

- Privileged
- Baseline
- Restricted

## Privileged

-

When you set the label `pod-security.kubernetes.io/enforce: privileged` on a namespace, you are allowing the most permissive security settings. Pods in this namespace can use capabilities that would otherwise be restricted under stricter policies. Specifically, the "privileged" policy allows:

- Use of host namespaces (like PID, IPC, and network).
- Use of host ports.
- Running as privileged containers.
- Allowing root file system writes.
- Skipping certain security configurations like AppArmor or SELinux.

This setting effectively disables most security restrictions, providing maximum flexibility for pods but at the cost of reduced security. It is generally recommended only for trusted workloads that need these capabilities.

## Applying the Privileged Policy

To apply the privileged policy to a namespace, you can add or update the label on that namespace. Here’s how you can do it:

Creating a New Namespace with Privileged Policy

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
  labels:
    pod-security.kubernetes.io/enforce: privileged
```

## Risks

Using the privileged policy level reduces the security of your cluster because it allows pods to perform actions that could potentially compromise the node or other pods. This is why it's critical to only use this policy for trusted workloads and in environments where security trade-offs are acceptable.

In summary, the `pod-security.kubernetes.io/enforce: privileged` label allows a namespace to bypass many security restrictions, including the use of host ports. However, it's important to balance this flexibility with the potential security risks involved.
