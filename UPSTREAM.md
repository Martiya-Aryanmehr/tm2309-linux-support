# Upstream Submission and Review History

## Current Status

- Patch version: v2
- Mailing list: platform-driver-x86
- Status: Awaiting review
- Last status check: 2026-11-26
- Mainline inclusion: Not accepted

## Patch Submission

### v1

- Date: 2026-11-22
- Subject:
  `[PATCH 1/1] platform/x86: bitland-mifs-wmi: add profile support for Xiaomi TM2309`
- Archive:
  https://lore-kernel.gnuweeb.org/platform-driver-x86/20261122163658.23920-1-Martiya.ar%40gmail.com/T/

### Maintainer Feedback

On 2026-11-23, Ilpo Järvinen requested that the TM2309
profile definitions and DMI entry be placed in
alphanumerically increasing order within their respective
structures.

### v2

- Sent: 2026-11-25 22:33:22 UTC
- Subject:
  `[PATCH v2 1/1] platform/x86: bitland-mifs-wmi: add profile support for Xiaomi TM2309`

Changes made in response to the v1 review:

- Moved the TM2309 profile definitions before TM2424.
- Moved the TM2309 DMI entry before TM2424.

The v2 patch was sent to the platform-driver-x86 mailing list.

No reply to v2 was found in the connected mailbox
during the status check on 2026-11-26.

## Review Guidelines

- Do not describe the patch as accepted until explicit
  maintainer confirmation or an upstream commit is found.
- Preserve every review comment and the corresponding response.
- Do not silently modify an already submitted patch.
- Prepare a new patch revision when requested changes
  require another submission.
- Keep local development changes separate from upstream
  patch revisions.

## Submitted Patch

The exact submitted v2 patch is maintained at:

`patches/0001-platform-x86-bitland-mifs-wmi-add-profile-support-for-xiaomi-tm2309.patch`
