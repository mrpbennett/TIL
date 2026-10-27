# Talos: Upgrading the OS Is Not Part of apply-config

After a `talosctl` client bump you may see this warning on every command:

```
WARNING: 192.168.7.1: server version 1.14.2 is older than client version 1.14.5
```

It is harmless, but it means the client is ahead of the nodes. In my case the
cause was `talosctl = "latest"` in `mise.toml`, so the client moved on its own
while the nodes stayed put.

You can fix it by pinning the client to the node version, or by upgrading the
nodes. The catch with upgrading is that editing the installer image in your
machine config and applying it does **not** upgrade the running OS. `apply-config`
only pushes config. The `installer.image` value is just what Talos uses the next
time it installs or upgrades. The OS upgrade is its own command.

## Find the schematic ID

If you build your image with the Image Factory (for extensions like
`iscsi-tools` for Longhorn), the schematic ID is the hex string in your
installer image:

```yaml
installer:
  image: factory.talos.dev/metal-installer/<schematic-id>:v1.14.2
```

You can also regenerate it from the schematic file. The ID is a hash of the
contents, so the same file always gives the same ID:

```bash
curl -X POST --data-binary @schematic.yaml https://factory.talos.dev/schematics
```

Note the path is `metal-installer`, not `installer`.

## Upgrade one node at a time

Bump the tag in the machine config patch first so git matches reality, then
upgrade each node. Keep the same schematic ID and change only the version:

```bash
talosctl upgrade --nodes 192.168.7.1 \
  --image factory.talos.dev/metal-installer/<schematic-id>:v1.14.5
```

The node reboots. Wait for it to be healthy before starting the next one:

```bash
kubectl get nodes                       # Ready, OS-IMAGE shows the new version
kubectl get volumes.longhorn.io -n longhorn-system   # all healthy, none degraded
```

On a cluster where every node is a control plane, going one at a time keeps etcd
quorum. If you run Longhorn, waiting for replicas to rebuild matters too,
otherwise taking down the next node can take volumes offline.

## Verify it actually happened

`kubectl get nodes -o wide` is a quick check. The `OS-IMAGE` column should show
the new Talos version. If it still shows the old one, the config was applied but
the OS was never upgraded.
