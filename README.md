# Description

The `aerialls/renovate-calver` repository contains two tags using [CalVer](https://calver.org/).

* 2023.12.26.0
* 2024.02.21.1

This repository is configured to include the GitLab CI file from the first repository. A custom versioning is configured to support the CalVer format in `gitlab-tags`.

```json
  "packageRules": [
    {
      "matchDatasources": ["gitlab-tags"],
      "versioning": "regex:^(?<major>\\d+)\\.(?<minor>\\d+)\\.(?<patch>\\d+)\\.(?<build>\\d+)$"
    }
  ],
```

Starting with version 37.194.4 (and probably from MR [!27383](https://github.com/renovatebot/renovate/pull/27383)), the configuration does not work anymore.

## Logs on 37.194.3 (working behavior)

```
DEBUG: packageFiles with updates (repository=xxx, baseBranch=master)
"config": {
 "gitlabci-include": [
   {
     "deps": [
       {
         "datasource": "gitlab-tags",
         "depName": "my-gitlab-repository",
         "depType": "repository",
         "currentValue": "2023.12.26.0",
         "registryUrls": ["https://internal-gitlab-server.com"],
         "updates": [
           {
             "bucket": "major",
             "newVersion": "2024.02.21.1",
             "newValue": "2024.02.21.1",
             "releaseTimestamp": "2024-02-21T11:53:46.000Z",
             "newMajor": 2024,
             "newMinor": 2,
             "updateType": "major",
             "branchName": "renovate/my-gitlab-repository-2024.x"
           }
         ],
         "packageName": "my-gitlab-repository",
         "versioning": "regex:^(?<major>\\d+)\\.(?<minor>\\d+)\\.(?<patch>\\d+)\\.(?<build>\\d+)$",
         "warnings": [],
         "sourceUrl": "https://internal-gitlab-server.com/my-gitlab-repository",
         "registryUrl": "https://internal-gitlab-server.com",
         "currentVersion": "2023.12.26.0",
         "isSingleVersion": true,
         "fixedVersion": "2023.12.26.0"
       }
     ],
     "packageFile": ".gitlab-ci.yml"
   }
 ]
}
```

The `currentValue` and `currentVersion` are the same and the new GitLab tag is found.

## Logs on 37.194.4 (non-working behavior)

```
DEBUG: packageFiles with updates (repository=xxx, baseBranch=master)
 "config": {
   "gitlabci-include": [
     {
       "deps": [
         {
           "datasource": "gitlab-tags",
           "depName": "my-gitlab-repository",
           "depType": "repository",
           "currentValue": "2023.12.26.0",
           "registryUrls": ["https://internal-gitlab-server.com"],
           "updates": [],
           "packageName": "my-gitlab-repository",
           "versioning": "regex:^(?<major>\\d+)\\.(?<minor>\\d+)\\.(?<patch>\\d+)\\.(?<build>\\d+)$",
           "warnings": [],
           "sourceUrl": "https://internal-gitlab-server.com/my-gitlab-repository",
           "registryUrl": "https://internal-gitlab-server.com",
           "currentVersion": "2023.12.26",
           "skipReason": "invalid-version"
         }
       ],
       "packageFile": ".gitlab-ci.yml"
     }
   ]
 }
```

The `currentValue` and `currentVersion` are not the same anymore. The `currentVersion` is missing the ending patch `.0`. The new GitLab tag is not found.
