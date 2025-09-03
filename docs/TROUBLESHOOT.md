# AEM Edge Delivery Services Troubleshooting Log

## Problem

Following the AEM tutorial at https://www.aem.live/developer/tutorial but getting 400 Bad Request errors when trying to access the preview environment.

## Setup Details

- **Repository**: https://github.com/raydot/aem_tutorial
- **Expected Preview URL**: https://main--aem_tutorial--raydot.aem.page/
- **Expected Live URL**: https://main--aem_tutorial--raydot.aem.live/
- **Tutorial Being Followed**: https://www.aem.live/developer/tutorial

## Current Status

- ✅ GitHub repository created (duplicated from tutorial, not templated)
- ✅ AEM Code Sync GitHub App installed and configured
- ✅ AEM CLI installed (`@adobe/aem-cli`)
- ✅ Local development server working (`aem up` command successful)
- ❌ Preview URL returning 400 Bad Request
- ❌ Missing `fstab.yaml` file in repository root

## Error Output from CLI

```
~/Documents/Dave/aem_tutorial on  main took 10s  aem up
    ___    ________  ___                          __      __ v16.10.47
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server v16.10.47
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem_tutorial--raydot.aem.page
opening default browser: http://localhost:3000/
warn: [0] Proxy GET request to https://main--aem_tutorial--raydot.aem.page/: 400 (text/plain)
warn: [2] Proxy GET request to https://main--aem_tutorial--raydot.aem.page/.well-known/appspecific/com.chrome.devtools.json: 400 (text/plain)
warn: [3] Proxy GET request to https://main--aem_tutorial--raydot.aem.page/: 400 (text/plain)
warn: [4] Proxy GET request to https://main--aem_tutorial--raydot.aem.page/.well-known/appspecific/com.chrome.devtools.json: 400 (text/plain)
```

## Root Cause Analysis

The 400 Bad Request error is occurring because the repository is missing the `fstab.yaml` file that defines the content source for Edge Delivery Services. This file tells AEM where to pull content from.

## Key Insights from Documentation Research

1. **fstab.yaml is mandatory**: This file defines content source mountpoints for Edge Delivery Services
2. **Content vs Code separation**: GitHub holds code/styling, external source holds content
3. **Tutorial uses Google Drive**: The specific tutorial being followed uses Google Drive as the content source
4. **AEM Code Sync working**: The CLI proxy setup is correct, the issue is content configuration

## Required fstab.yaml Configuration

Based on the tutorial, the file should initially contain:

```yaml
mountpoints:
  /: https://drive.google.com/drive/folders/1MGzOt7ubUh3gu7zhZIPb7R7dyRzG371j
```

(This points to the tutorial's example content)

## Next Steps to Resolve

1. Create `fstab.yaml` in repository root with above content
2. Commit and push to GitHub
3. Wait for propagation (30-60 seconds)
4. Test preview URL again
5. Follow tutorial's Google Drive setup steps for custom content

## Alternative Approaches to Consider

- Document Authoring tutorial (da.live) - uses different content approach
- Universal Editor tutorial - uses AEM authoring instance
- Check if using Helix 5 architecture (newer, doesn't require fstab.yaml)

## Documentation Sources Referenced

- https://www.aem.live/developer/tutorial (main tutorial)
- https://www.aem.live/docs/faq (fstab.yaml explanation)
- https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/edge-delivery-sites/configure-content-source
