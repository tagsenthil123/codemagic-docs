---
description: Upload files to be included in your build artifacts ZIP file
title: Upload custom artifacts
weight: 3
---

You can use a custom script to upload custom artifacts, such as screenshots, to `$FCI_EXPORT_DIR` and include them in the build artifacts ZIP file generated by Codemagic.

For example, add this **post-test** script to copy screenshots to `$FCI_EXPORT_DIR`.

    #!/usr/bin/env sh

    cp  -r build/screenshots $FCI_EXPORT_DIR/screenshots
