# Goconfig

![Logo](logo.png)

[![Build Status](https://app.travis-ci.com/fulldump/goconfig.svg?branch=master)](https://app.travis-ci.com/github/fulldump/goconfig)
[![Go Report Card](https://goreportcard.com/badge/github.com/fulldump/goconfig)](https://goreportcard.com/report/github.com/fulldump/goconfig)
[![Go Reference](https://godoc.org/github.com/fulldump/goconfig?status.svg)](https://godoc.org/github.com/fulldump/goconfig)
[![Maintainability](https://api.codeclimate.com/v1/badges/d3f50778ac8598d4438f/maintainability)](https://codeclimate.com/github/fulldump/goconfig/maintainability)

`goconfig` is a lightweight library that populates your Go structs from command
line flags, environment variables and JSON configuration files. It aims to make
configuration straightforward while keeping your code idiomatic.

## Features

- Unified configuration from flags, environment variables and JSON files
- Hierarchical keys using struct fields
- Supports arrays and most native flag types
- Auto-generated `-help` with usage information

## Installation

```bash
go get github.com/fulldump/goconfig
```

## Quick Start

Define your configuration struct with descriptive tags:

```go
type myconfig struct {
        Name      string `usage:"The name of something"`
        EnableLog bool   `usage:"Enable logging into logdb" json:"enable_log"`
        MaxProcs  int    `usage:"Maximum number of procs"`
        UsersDB   db
        LogDB     db
}

type db struct {
        Host string `usage:"Host where db is located"`
        User string `usage:"Database user"`
        Pass string `usage:"Database password"`
}
```

Provide defaults and read the configuration:

```go
c := &myconfig{
        EnableLog: true,
        UsersDB: db{
                Host: "localhost",
                User: "root",
                Pass: "123456",
        },
}

goconfig.Read(c)
```

Running your program with `-help` prints automatically generated help text:

```
Usage of example:
  -enablelog
        Enable logging into logdb (default true)
  -logdb.host string
        Host where db is located (default "localhost")
  -logdb.pass string
        Database password (default "123456")
  -logdb.user string
        Database user (default "root")
  -maxprocs int
        Maximum number of procs
  -name string
        The name of something
  -usersdb.host string
        Host where db is located
  -usersdb.pass string
        Database password
  -usersdb.user string
        Database user
```

## Supported Types

`goconfig` supports the basic types from the `flag` package plus arrays and
nested structs:

- bool
- float64
- int64
- int
- string
- uint64
- uint
- struct (hierarchical keys)
- array (any type)

The `time.Duration` type is currently not supported.

## Built-in Flags

### `-help`

Uses the standard `flag` behaviour to display help.

### `-config`

Read configuration from a JSON file. Given the previous configuration structure,
a sample `config.json` looks like:

```json
{
  "name": "Fulanito",
  "usersdb": {
    "host": "localhost",
    "user": "admin",
    "pass": "123"
  }
}
```

Configuration precedence (highest to lowest):
1. Command line arguments
2. JSON config file
3. Environment variables
4. Default values

## Contributing

Contributions are welcome! Feel free to fork the repository, submit pull
requests, or [open an issue](https://github.com/fulldump/goconfig/issues) if you
encounter problems or have suggestions.

### Testing

Run the full test suite with:

```bash
make
```

### Example Project

This repository includes a small example. Build it with:

```bash
make example
```

## License

Goconfig is released under the [MIT License](LICENSE).
