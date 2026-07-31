# Brainlift Desktop Installer

The repository's current macOS launcher installs the published `anki-release`
package. That path cannot prove a local feature branch because it may resolve to
an upstream wheel. The Brainlift proof build keeps the existing launcher shape
but bundles the local `anki` and `aqt` wheels and locks all other dependencies.

Build the proof installer on Apple Silicon:

```shell
ts/lib/editable/build-brainlift.sh
```

The build:

- creates the local `anki` and `aqt` wheels;
- embeds them in `Anki Brainlift.app`;
- records the exact Git commit in the app resources;
- gives the app the distinct `dev.techmex.brainlift` bundle identifier;
- performs an isolated first-install smoke that imports the packaged Rust bridge
  and Brainlift desktop surface; and
- creates `out/brainlift-installer/Anki-Brainlift.dmg` plus SHA-256 checksums.

The app uses its bundled `uv` executable to create a per-user runtime on first
launch. The local feature wheels never resolve from a package index. Python,
PyQt, and other unchanged third-party dependencies are downloaded from their
normal indexes, matching the upstream launcher's dependency model.

The proof DMG is ad-hoc signed for local testing. It is not Developer ID signed
or notarized, so it is not a public production release artifact.

Latest reconciled-branch proof (2026-07-30): the installer build, isolated first
install smoke, DMG creation, and 97 desktop/Qt tests passed at commit
`a62d39eaa20587b28fa25a2c4b816a1aa932287d`. The DMG SHA-256 is
`74b43352b70322e62bfa2d78751063b9d1c138930b85831051282be27ec12ae3`.

The builder refuses tracked and untracked worktree changes. Ignored build
outputs remain allowed. The embedded commit therefore identifies the exact
source used to build the bundled local wheels.
