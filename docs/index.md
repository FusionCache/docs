# FusionCache Overview

FusionCache is an object cache for storing, searching and retrieving data. Queries and responses are handled by REST and WebSocket interfaces, with a dedicated WebSocket interface for bulk data.

The priority is reducing latency, particularly for read queries. A read query is one which does not change data, such `GET` or `FIND`. The opposite to this is a write query which changes data, such as `STORE` or `UPDATE`.

{: .note}

From here onwards, FusionCache is referred to as just Fusion.



## Design

Fusion's engine is designed to priotise read queries, maxing all cores if required. The engine is fully asychronous, including the network and query engine code to maximise CPU resources. 



## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
