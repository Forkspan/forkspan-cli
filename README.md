# Forkspan CLI

Forkspan CLI runs the sandbox service on a machine you control. The command is `forkspan`.
This public repository distributes signed Linux and macOS releases. Source development takes
place in the Forkspan application repository.

## Download the latest release

Choose a target matching your machine: `linux-x86_64`, `linux-aarch64`,
`macos-x86_64`, or `macos-aarch64`. Linux needs systemd and Docker; macOS needs
Docker and a logged-in user session. The CLI also needs `git` and `df` on the host.

```sh
target=linux-x86_64 # change this for your machine
base=https://github.com/Forkspan/forkspan-cli/releases/latest/download
curl -fLO "$base/forkspan-cli-$target.tar.gz"
curl -fLo release.json "$base/forkspan-cli-$target.release.json"
curl -fLo release.json.sig "$base/forkspan-cli-$target.release.json.sig"
tar -xzf "forkspan-cli-$target.tar.gz"
openssl pkeyutl -verify -pubin -inkey release-signing-key.pem \
  -rawin -in release.json -sigfile release.json.sig
./forkspan --version
```

Run these commands in a new directory after downloading
[the release signing public key](release-signing-key.pem) into that directory.
The archive includes `THIRD-PARTY-NOTICES.txt`. Keep the extracted files and the
detached metadata together when you run `forkspan install`; installation checks
the signature, target, executable, and bundled engine before it registers the machine.

Installation requires a one-use registration token issued by your Forkspan workspace.
Pass it on standard input. For example, on Linux:

```sh
printf '%s\n' "$TOKEN" | sudo ./forkspan install --noninteractive \
  --base-url https://your-forkspan.example --name build-01 --token-stdin
```

On macOS, run the same command without `sudo`. After installation, use
`forkspan doctor` and `forkspan status` to inspect the machine.
