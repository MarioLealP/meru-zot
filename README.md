# Meru Zot

The image registry of **Meru**, a small in-house container platform (CaaS) on k3s: [Zot](https://zotregistry.dev),
deployed by Argo CD from this repo into namespace `zot` (the Application is in
[Meru/meru](https://git.clockmost.com/Meru/meru) `platform/zot.yaml`).

- **Our images:** `meru/<app>`, pushed only by that app's builds - one push login per app, allowed on its own
  repository only (`config.json` `accessControl`); everyone may read. Signatures live next to the images. `meru/**` is
  Charon's delete (D112): the most specific match wins, so it reaches only repositories with no entry of their own, the
  retired apps. Every live app needs its own entry.
- **Pull-through mirror** for docker.io, ghcr.io and quay.io: copies only what the cluster pulls, so every image the
  cluster runs comes through here and gets scanned.
- **CVE scans** with Trivy (database refreshed daily).
- **Retention:** mirror copies are kept 90 days after their last pull (the 3 newest per repository always); our
  images keep the 10 newest plus anything pulled in the last 30 days.

The push logins are a hand-made Secret (`zot-htpasswd`), never in this repo. Data lives on the server's `hdd`
storage class.

## License

MIT - see `LICENSE`.
