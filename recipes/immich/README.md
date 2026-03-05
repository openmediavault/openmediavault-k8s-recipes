# Immich

High performance self-hosted photo and video management solution.

The app can be reached at `https://immich.<FQDN>:8443` after deployment.

## Prerequisites

Before deploying this recipe, create the following **shared folders** in
`Storage | Shared Folders` in the openmediavault Workbench:

| Variable                  | Purpose                                                                                                                                 |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| `librarySharedFolderName` | Stores the Immich photo/video library.                                                                                                  |
| `importSharedFolderName`  | Optional. An existing folder whose contents can be imported as an [external library](https://immich.app/docs/guides/external-library/). |
| `dbSharedFolderName`      | Stores the PostgreSQL database files.                                                                                                   |
| `mlCacheSharedFolderName` | Caches the machine-learning models (face recognition, smart search).                                                                    |

## Configuration

Open the recipe and adjust the variables at the top of the file:

```yaml
{% set runAsUser   = 'nobody' %}    # System user that runs the Immich containers.
{% set runAsDbUser = 'nobody' %}    # System user that runs the PostgreSQL container.
{% set runAsGroup  = 'users' %}     # Group shared by all containers and the host folders.
{% set dbPassword  = 'immich' %}    # Password for the PostgreSQL `immich` database user.
                                    # Change this to a strong, unique password.

{% set librarySharedFolderName = '<MODIFY>' %}    # Shared folder for the photo/video library.
{% set importSharedFolderName  = '<MODIFY>' %}    # Shared folder for external library imports.
{% set dbSharedFolderName      = '<MODIFY>' %}    # Shared folder for the PostgreSQL data directory.
{% set mlCacheSharedFolderName = '<MODIFY>' %}    # Shared folder for the ML model cache.
```

Replace every `<MODIFY>` with the **exact name** of the shared folder you
created in the previous step. `librarySharedFolderName`, `importSharedFolderName` and `mlCacheSharedFolderName` and have to be owned/readable by the user set with `runAsUser`.
`dbSharedFolderName` has to be owned/readable by the user set in `runAsDbUser`.



## First start

Open the Immich web interface at `https://immich.<FQDN>:8443` and create
your admin account.

## Machine learning

The machine-learning pod downloads AI models on first start and stores
them in `mlCacheSharedFolderName`. Keeping the models on a shared folder
avoids re-downloading them after a pod restart.
