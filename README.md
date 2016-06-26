![YARA-logo](https://raw.githubusercontent.com/maliceio/malice-yara/master/logo.png)

malice-yara
===========

[![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org) [![Docker Stars](https://img.shields.io/docker/stars/malice/yara.svg)](https://hub.docker.com/r/malice/yara/) [![Docker Pulls](https://img.shields.io/docker/pulls/malice/yara.svg)](https://hub.docker.com/r/malice/yara/)

Malice Yara Plugin

This repository contains a **Dockerfile** of **malice/yara** for [Docker](https://www.docker.io/)'s [trusted build](https://hub.docker.com/r/malice/yara/) published to the public [DockerHub](https://hub.docker.com/).

### Dependencies

-	[gliderlabs/alpine](https://hub.docker.com/_/gliderlabs/alpine/)

### Installation

1.	Install [Docker](https://www.docker.io/).
2.	Download [trusted build](https://hub.docker.com/r/malice/yara/) from public [DockerHub](https://hub.docker.com): `docker pull malice/yara`

### Usage

```
docker run --rm -v /path/to/rules:/rules:ro malice/yara FILE
```

#### Or link your own malware folder

```bash
$ docker run -v /path/to/malware:/malware:ro -v /path/to/rules:/rules:ro malice/yara FILE

Usage: yara [OPTIONS] COMMAND [arg...]

Malice YARA Plugin

Version: v0.1.0, BuildTime: 20160214

Author:
  blacktop - <https://github.com/blacktop>

Options:
  --verbose, -V		verbose output
  --rethinkdb value	rethinkdb address for Malice to store results [$MALICE_RETHINKDB]
  --post, -p		POST results to Malice webhook [$MALICE_ENDPOINT]
  --proxy, -x		proxy settings for Malice webhook endpoint [$MALICE_PROXY]
  --table, -t		output as Markdown table
  --rules value		YARA rules directory (default: "/rules")
  --help, -h		show help
  --version, -v		print the version

Commands:
  help	Shows a list of commands or help for one command

Run 'yara COMMAND --help' for more information on a command.
```

This will output to stdout and POST to malice results API webhook endpoint.

### Sample Output JSON:

```json
{
  "yara": {
    "matches": [
      {
        "Rule": "_First_Publisher_Graphics_format_",
        "Namespace": "malice",
        "Tags": [],
        "Meta": {
          "description": "First Publisher Graphics format"
        },
        "Strings": [
          {
            "Name": "$1",
            "Offset": 2425,
            "Data": "AAAAAAAAHwE="
          }
        ]
      }
    ]
  }
}
```

### Sample FILTERED Output JSON:

```bash
$ cat JSON_OUTPUT | jq '.[][][] .Rule'

"_Microsoft_Visual_Cpp_v50v60_MFC_"
"_Borland_Delphi_v60__v70_"
"_dUP_v2x_Patcher__wwwdiablo2oo2cjbnet_"
"_Free_Pascal_v106_"
"_Armadillo_v171_"
```

### Sample Output STDOUT (Markdown Table):

---

#### yara

| Rule                                    | Description                                 | Offset | Data                                 | Tags |
|-----------------------------------------|---------------------------------------------|--------|--------------------------------------|------|
| *Microsoft_Visual_Cpp_v50v60_MFC*       | Microsoft Visual C++ v5.0/v6.0 (MFC)        | 5204   | U��                                  |      |
| *Borland_Delphi_v60\__v70*              | Borland Delphi v6.0 - v7.0                  | 5204   | U��                                  |      |
| *dUP_v2x_Patcher\__wwwdiablo2oo2cjbnet* | dUP v2.x Patcher --> www.diablo2oo2.cjb.net | 78     | This program cannot be run in DOS mo |      |
| *Free_Pascal_v106*                      | Free Pascal v1.06                           | 14866  | ��@O�k                               |      |
| *Armadillo_v171*                        | Armadillo v1.71                             | 23110  | U��j�h b@h�[@d�                      |      |

---

### To write results to [RethinkDB](https://rethinkdb.com)

```bash
$ docker volume create --name malice
$ docker run -d -p 28015:28015 -p 8080:8080 -v malice:/data --name rethink rethinkdb
$ docker run --rm -v /path/to/malware:/malware:ro --link rethink:rethink malice/yara -t FILE
```

### To Run on OSX

-	Install [Homebrew](http://brew.sh)

```bash
$ brew install caskroom/cask/brew-cask
$ brew cask install virtualbox
$ brew install docker
$ brew install docker-machine
$ docker-machine create --driver virtualbox malice
$ eval $(docker-machine env malice)
```

### Documentation

### Issues

Find a bug? Want more features? Find something missing in the documentation? Let me know! Please don't hesitate to [file an issue](https://github.com/maliceio/malice-av/issues/new) and I'll get right on it.

### Credits

### License

MIT Copyright (c) 2016 **blacktop**
