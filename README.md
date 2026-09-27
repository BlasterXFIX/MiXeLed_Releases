# MiXeLeD Releases

Public release channel for the MiXeLeD desktop application.

The source code and production database are **not** stored in this repository.
Only published application packages and the public update manifest belong here.

## Release procedure

1. Build/publish MiXeLeD in Release mode.
2. Zip the **contents of the publish folder** as:
   `MiXeLeD-X.Y.Z.zip`
3. Create a GitHub Release with tag:
   `vX.Y.Z`
4. Upload the ZIP as a release asset.
5. Calculate SHA-256 on the final ZIP:

   ```powershell
   Get-FileHash .\MiXeLeD-X.Y.Z.zip -Algorithm SHA256
   ```

6. Update `latest.json` only after the release asset is uploaded and the SHA-256 is known.

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
