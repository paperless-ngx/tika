# tika-docker

[![build and publish](https://github.com/paperless-ngx/tika/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/paperless-ngx/tika/actions/workflows/docker-publish.yml)

This fork exists to add the `arm64` and `armv7` architectures to the tika docker image. In theory these images are identical to those of [apache/tika-docker](https://github.com/apache/tika-docker). This enables users of [paperless-ngx](https://github.com/paperless-ngx/paperless-ngx/) to run on more devices, such as Raspberry Pis

⚠ This image is currently being used in the main repo's docker example files found [here](https://github.com/paperless-ngx/paperless-ngx/tree/main/docker/compose).

---

This repo is used to create convenience Docker images for Apache Tika Server published as [apache/tika](https://hub.docker.com/r/apache/tika) on DockerHub by the [Apache Tika](http://tika.apache.org) Dev team

The images create a functional Apache Tika Server instance that contains the latest Ubuntu running the appropriate version's server on Port 9998 using Java 8 (until version 1.20), Java 11 (1.21 and 1.24.1), Java 14 (until 1.27/2.0.0), Java 16 (for 2.1.0), and Java 17 LTS for newer versions.

There is a minimal version, which contains only Apache Tika and it's core dependencies, and a full version, which also includes dependencies for the GDAL and Tesseract OCR parsers. To balance showing functionality versus the size of the full image, this file currently installs the language packs for the following languages:

- English
- French
- German
- Italian
- Spanish.

To install more languages simply update the apt-get command to include the package containing the language you required, or include your own custom packs using an ADD command.

## Available Tags

Below are the most recent 2.x series tags:

- `2.1.0`: Apache Tika Server 2.1.0 (Minimal)
- `2.1.0-full`: Apache Tika Server 2.1.0 (Full)

Below are the most recent 1.x series tags:

- `1.27`: Apache Tika Server 1.27 (Minimal)
- `1.27-full`: Apache Tika Server 1.27 (Full)

You can see a full set of tags for historical versions [here](https://hub.docker.com/r/apache/tika/tags?page=1&ordering=last_updated).

## Usage

### Default

You can pull down the version you would like using:

    docker pull iwishiwasaneagle/apache-tika-arm:<tag>

Then to run the container, execute the following command:

    docker run -d -p 9998:9998 iwishiwasaneagle/apache-tika-arm:<tag>

Where <tag> is the DockerHub tag corresponding to the Apache Tika Server version - e.g. 1.23, 1.22, 1.23-full, 1.22-full.

NOTE: The latest and latest-full tags are explicitly set to the latest released version when they are published.

## Building

To build the image from scratch, simply invoke:

    docker build -t 'apache/tika' github.com/apache/tika-docker

You can then use the following command (using the name you allocated in the build command as part of -t option):

    docker run -d -p 9998:9998 apache/tika

## More Information

For more infomation on Apache Tika Server, go to the [Apache Tika Server documentation](https://cwiki.apache.org/confluence/display/TIKA/TikaServer).

For more information on Apache Tika, go to the official [Apache Tika](http://tika.apache.org) project website.

To meet up with others using Apache Tika, consider coming to one of the [Apache Tika Virtual Meetups](https://www.meetup.com/apache-tika-community/).

For more information on the Apache Software Foundation, go to the [Apache Software Foundation](http://apache.org) website.

## Authors

Apache Tika Dev Team (dev@tika.apache.org)

## Contributors

There have been a range of [contributors](https://github.com/apache/tika-docker/graphs/contributors) on GitHub and via suggestions, including:

- [@grossws](https://github.com/grossws)
- [@arjunyel](https://github.com/arjunyel)
- [@mpdude](https://github.com/mpdude)
- [@laszlocsontosuw](https://github.com/laszlocsontosuw)

## Licence

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Disclaimer

It is worth noting that whilst these Docker images download the binary JARs published by the Apache Tika Team on the Apache Software Foundation distribution sites, only the source release of an Apache Software Foundation project is an official release artefact. See [Release Distribution Policy](https://www.apache.org/dev/release-distribution.html) for more details.
