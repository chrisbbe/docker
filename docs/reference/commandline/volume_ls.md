<!--[metadata]>
+++
title = "volume ls"
description = "The volume ls command description and usage"
keywords = ["volume, list"]
[menu.main]
parent = "smn_cli"
+++
<![end-metadata]-->

# volume ls

```markdown
Usage:  docker volume ls [OPTIONS]

List volumes

Aliases:
  ls, list

Options:
  -f, --filter value   Provide filter values (i.e. 'dangling=true') (default [])
                       - dangling=<boolean> a volume if referenced or not
                       - driver=<string> a volume's driver name
                       - name=<string> a volume's name
      --format string  Pretty-print volumes using a Go template
      --help           Print usage
  -q, --quiet          Only display volume names
```

Lists all the volumes Docker knows about. You can filter using the `-f` or `--filter` flag. Refer to the [filtering](#filtering) section for more information about available filter options.

Example output:

    $ docker volume create --name rosemary
    rosemary
    $docker volume create --name tyler
    tyler
    $ docker volume ls
    DRIVER              VOLUME NAME
    local               rosemary
    local               tyler

## Filtering

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more
than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

The currently supported filters are:

* dangling (boolean - true or false, 0 or 1)
* driver (a volume driver's name)
* name (a volume's name)

### dangling

The `dangling` filter matches on all volumes not referenced by any containers

    $ docker run -d  -v tyler:/tmpwork  busybox
    f86a7dd02898067079c99ceacd810149060a70528eff3754d0b0f1a93bd0af18
    $ docker volume ls -f dangling=true
    DRIVER              VOLUME NAME
    local               rosemary

### driver

The `driver` filter matches on all or part of a volume's driver name.

The following filter matches all volumes with a driver name containing the `local` string.

    $ docker volume ls -f driver=local
    DRIVER              VOLUME NAME
    local               rosemary
    local               tyler

### name

The `name` filter matches on all or part of a volume's name.

The following filter matches all volumes with a name containing the `rose` string.

    $ docker volume ls -f name=rose
    DRIVER              VOLUME NAME
    local               rosemary

## Formatting

The formatting options (`--format`) pretty-prints volumes output
using a Go template.

Valid placeholders for the Go template are listed below:

Placeholder   | Description
--------------|------------------------------------------------------------------------------------------
`.Name`       | Network name
`.Driver`     | Network driver
`.Scope`      | Network scope (local, global)
`.Mountpoint` | Whether the network is internal or not.
`.Labels`     | All labels assigned to the volume.
`.Label`      | Value of a specific label for this volume. For example `{{.Label "project.version"}}`

When using the `--format` option, the `volume ls` command will either
output the data exactly as the template declares or, when using the
`table` directive, includes column headers as well.

The following example uses a template without headers and outputs the
`Name` and `Driver` entries separated by a colon for all volumes:

```bash
$ docker volume ls --format "{{.Name}}: {{.Driver}}"
vol1: local
vol2: local
vol3: local
```

## Related information

* [volume create](volume_create.md)
* [volume inspect](volume_inspect.md)
* [volume rm](volume_rm.md)
* [Understand Data Volumes](../../tutorials/dockervolumes.md)
