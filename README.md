t-rex
=====

[![Travis build Status](https://travis-ci.org/pka/t-rex.svg?branch=master)](https://travis-ci.org/pka/t-rex) [![Appveyor build status](https://ci.appveyor.com/api/projects/status/o60e9bu97i49lxyf?svg=true)](https://ci.appveyor.com/project/pka/t-rex)


t-rex is a vector tile server specialized on publishing [MVT tiles](https://github.com/mapbox/vector-tile-spec/tree/master/2.1)
from a PostGIS database.

An extensible design allows future support for more data sources (e.g. OGR), custom tile
grids with other reference systems than Spherical Mercator and additional output formats like
JSON.

Presentations
-------------

* Workshop "Vector Tiles", GEOSummit Bern 7.6.16: [slides](doc/t-rex_vector_tile_server.pdf)


Usage
-----

    t_rex serve --dbconn postgresql://pi@localhost/osm2vectortiles

Tiles are then served at `http://localhost:6767/{layer}/{z}/{x}/{y}.pbf`

A list of all detected layers is available at [http://localhost:6767/](http://localhost:6767/)

Use a tile cache:

    t_rex serve --dbconn postgresql://pi@localhost/osm2vectortiles --cache /tmp/mvtcache

Generate a configuration template:

    t_rex genconfig --dbconn postgresql://pi@localhost/osm2vectortiles

Run server with configuration file:

    t_rex serve --config osm2vectortiles.cfg


Installation
------------

Pre-built binaries are available for 64 bit Linux and Windows. Download your binary from [github.com/pka/t-rex/releases](https://github.com/pka/t-rex/releases) and unpack it.

`t_rex` is an executable with very few dependencies, essentially `libgcc_s.so.1` on Linux and `msvcr120.dll` on Windows. If `msvcr120.dll` is missing, install `vcredist_x64.exe` from [here](https://www.microsoft.com/download/details.aspx?id=40784).


MBTiles creation
----------------

To create MBTiles files with vector tiles from a local cache you can use [MBUtil](https://github.com/mapbox/mbutil).

Example:

    mb-util --image_format=pbf /tmp/mvtcache/streets streets.mbtiles


For developers
--------------

t-rex is written in [Rust](https://www.rust-lang.org/).

Build:

    cargo build

Run tests:

    cargo test

To run DB tests you have to set an environment variable with the [connection spec](https://github.com/sfackler/rust-postgres#connecting) first. Example:

     export DBCONN=postgresql://pi@localhost/natural_earth_vectors

Creating test database:

    # Set Postgresql environment variables when needed: PGHOST, PGPORT, PGUSER, PGPASSWORD
    cd src/test
    make

Run server:

    cargo run -- serve --dbconn postgresql://pi@%2Frun%2Fpostgresql/natural_earth_vectors


License
-------

t-rex is released under the MIT License.
