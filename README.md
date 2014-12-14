# chkbit

chkbit is a lightweight **bitrot detection tool**.

bitrot (a bit flipping in your data) can occur

- at a low level on the storage media through decay (hdd/sdd)
- at a high level in the os or firmware through bugs

chkbit is independent of the file system and can help you detect bitrot on you primary system, on backups and in the cloud.

## Installation

`npm i chkbit -g`

If you have `md5sum` (Linux/[Windows](http://gnuwin32.sourceforge.net/packages/coreutils.htm)) or `md5` (Mac) in your path it will be used in place of the slower nodejs module.

## Usage

Run `chkbit DIR` to create/update the chkbit index.

chkbit will

- create a `.chkbit` index in every subdirectory of the path it was given.
- update the index with md5 hashes for every file.
- report bitrot for files that rotted since the last run (check the exit status).

For other options see the CLI.

## Repair

chkbit cannot repair bitrot, its job is simply to detect it.

You should

- backup regularly.
- run chkbit *before* each backup.
- check for bitrot on the backup media.
- in case of bitrot *restore* from a checked backup.

## FAQ

### Should I run `chkbit` on my whole drive?

You would typically run it only on *content* that you keep for a long time (e.g. your pictures, music, videos).

### Why is chkbit placing the index in `.chkbit` files (vs a database)?

The advantage of the .chkbit files is that

- when you move a directory the index moves with it
- when you make a backup the index is also backed up

The disadvantage is that you get hidden `.chkbit` files in your content folders.

## API Usage

```
var chkbit=require('../lib/chkbit.js');

var opt={
  status: function(stat, file) { console.log(stat, file); },
  overwrite: false,
  verbose: false,
  readonly: false,
};
```

### verify

```
chkbit.verify(opt, "/path").then(function(res) {
  if (res) console.error("error: detected "+res+" file(s) with bitrot!");
  process.exit(res);
}).catch(...)
```

### delete

```
chkbit.del(opt, "/path").then(function() { console.log("removed."); });
```

Removes all index files.