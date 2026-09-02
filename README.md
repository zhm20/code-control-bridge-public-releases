# Code Control Bridge — Public Releases

This repository is the public binary-distribution surface for Code Control
Bridge. It contains no product source code. Product implementation, tests,
private operational evidence, credentials, and signing material remain private.

## Official locations

- Product home: <https://ccb.zhm20.com>
- Official download entry: <https://downloads.zhm20.com/ccb>
- Independent trust channel: <https://trust.zhm20.com/ccb>
- Support: `eric.zhm2018@gmail.com`

## Current commercial beta

- Version: `0.1.0-beta.33`
- Release: <https://github.com/zhm20/code-control-bridge-public-releases/releases/tag/v0.1.0-beta.33>
- Package: `0.1.0-beta.33.pkg`
- Package SHA-256: `7b7437d39970dc2e9d5b3343bedde50c8ad5e84804bce753dcb1e2fe13f16eb2`
- Buyer verifier SHA-256: `b7ceb7c5e9b6700ce89638cedf2dfd387de765bc8aa53de505a0816e0c9044fc`
- Release public-key SHA-256: `796d69af8f8ef6b74551573eded3c54474b02e6f1e464889dd068c1b116a8dec`

The package is not Apple-signed, notarized, or stapled. Before installation,
obtain the verifier and public-key fingerprints from the independent trust
channel, compare the downloaded verifier SHA-256, and then run the verifier
against the package, release statement, and detached signature.

Published release assets are immutable. Corrections use a new version; existing
tags and assets are never replaced.
