# immich-migrate-albums
Migrate album assets between immich accounts

**!!! This has only been tested on Ubuntu OSes, but should work on other linux based systems !!!**

<ins>I recommend using the new docker container [immich-migrate-tools](https://github.com/lveatch/immich-migrate-tools) for migrating albums.</ins>

## Overview
Exports album assets from one immich account to another account. This can be between two accounts on the same instance or seperate instances. Matching between accounts is done on asset name and created date/time (fileCreatedAt) attribute as determined by Immich with the hopes of dealing with possible duplicate asset names.

You must supply at least one album name to export, multiple are allowed during export.  Import will use album name from the export file.  
***Note:*** You must create the same album name in your targeted user account.

## Requirements:
- Each immich account requires an api key
   - Navigate to your Account settings
   - Expand **API Keys** section
   - Click **New API Key** button
   - Provide a name and click **Create**
   - Click **Copy to Clipboard**
   - Save to a secure location
   - Click **Done**
## Installing
- Download the tar.gz file and uncompress
- Edit the configuration files setting your http://immich:2283 sever name and appropiate API keys
  - immich.export.config
  - immich.import.config


### immich.export.config
```ini
server=http://localhost:2283

# from@domain.tld
apiKey=QKLLxISmpuvJAzCjxQbCWg1FRPsuI8ndvMyti9sQ

```

### immich.import.config
```ini
server=http://localhost:2283

# to@domain.tld
apiKey=bIln15UFJwZrKJy9TeNK8BASO7t4IS9Fmof8YByd9s

```

## Executing
### Exporting (this will not modify your immich data)
```
./export.albums -a 'album name 1' -a 'album name 2' [-f export.filename] [-c configfile]
```

ouput:
```
album id = cfc44db8-ae00-4d42-baf8-b99085e1622d
album id = caf3b2ab-046d-4044-9b8b-0d6bcf04d25d
```

### Importing
```
./import.albums [-f export.filename] [-c configfile]
```

output:
```
-=-=- Have the new albums been created in the target account? -=-=-

Control-C to exit. Enter to continue.

line 704 / 704


Assets added (failed) to albums:
    701 (      0) : another one
      3 (      0) : test

```

## Testing
You can edit the generated output **album.export** file reducing to test assets before comitting to a full run.

### How I tested.
1. I exported from a small (4000+) external library I have of scans from the in-laws.
2. I created a new test user on the same immich instance and created a new API key.
3. As admin, I created a new external library, owned by the new test user, a subset of that inital external library containing 899 images.
4. I scanned that new test library and waited for all jobs to complete.
5. Ran the import script.
6. Verified the album counts matched
7. Spot checked the results.
   
