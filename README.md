# MiXeLeD Releases

Public release channel for the MiXeLeD desktop application.

The source code and production database are **not** stored in this repository.
Only published application packages and the public update manifest belong here.

## Publish directly from Visual Studio

The main MiXeLeD repository contains:

- `Publish-MiXeLeD-Release.cmd`
- `tools/Publish-MiXeLeDRelease.ps1`

Open **Terminal** in Visual Studio at the repository root and run:

```powershell
.\Publish-MiXeLeD-Release.cmd
```

The script automatically:

1. Reads the version from `MiXeLeD/MiXeLeD.vbproj`.
2. Runs `dotnet publish -c Release`.
3. Creates `MiXeLeD-X.Y.Z.zip`.
4. Calculates the SHA-256 checksum.
5. Creates the GitHub Release `vX.Y.Z` and uploads the ZIP.
6. Updates `latest.json` only after the release package has uploaded successfully.

Local build output is written under `.release\X.Y.Z\` in the main source repository
and is ignored by Git.

### First-time setup on the development PC

GitHub CLI must be installed and authenticated:

```powershell
winget install --id GitHub.cli
gh auth login
```

After that, normal releases need only the one publish command above.

Optional release notes can be supplied from the Visual Studio terminal:

```powershell
.\Publish-MiXeLeD-Release.cmd -Notes "Description of this release"
```

To mark a release as mandatory:

```powershell
.\Publish-MiXeLeD-Release.cmd -Notes "Required update" -Mandatory
```

A published version is treated as immutable. If `vX.Y.Z` already exists, the
script stops and asks for a version bump. `-ForceRepublish` exists only for
recovering an incomplete/broken publication of the same version.

## Update manifest

Example:

```json
{
  "version": "1.0.86",
  "packageUrl": "https://github.com/BlasterXFIX/MiXeLed_Releases/releases/download/v1.0.86/MiXeLeD-1.0.86.zip",
  "sha256": "PUT_64_CHARACTER_SHA256_HERE",
  "mandatory": false,
  "notes": [
    "Description of the update"
  ]
}
```

MiXeLeD checks this manifest at startup. A package is installed only when the
manifest version is newer than the installed version and the downloaded ZIP
matches the configured SHA-256 checksum.

Version 1.0.85 is the bootstrap release for the remote-update mechanism.
A workstation still running 1.0.84 must receive 1.0.85 manually once.
