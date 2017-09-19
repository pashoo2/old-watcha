---
title: Watcha
---

Geo p2p(js WebRTC) social network.
==================================

**Deprecated**

 

>   **This project has deprecated**. Use
>   [Watcha2](https://watchasn.com)[instead.](https://watchasn.com) It based on
>   [IPFS](https://github.com/ipfs/js-ipfs)and
>   [OrbitDB](https://github.com/orbitdb/orbit-db), I think it fully consistent
>   with the Watcha’s philosophy.

 

It allows you to watch users around in real-time on the map, messaging with
them, filter by tags, create a video broadcasts using WebRTC.

 

### The key features:

-   User can watch what’s going around on the map page. Moves of other users are
    displayed on the map.

-   User can define a custom filter on settings page. It use tags to define
    which users can be shown on the map.

-   Chat with others by clicking on their markers.

-   Create and share a video broadcasts with your device’s camera by one button
    click. There is no a performance and bandwidth issues because the other user
    watching your broadcast can share it with other.

 

### The key technologies:

-   P2P connections and peer -to-server are based on the custom
    [PeerJS](http://peerjs.com/) build. Key feature of my build, that it
    parse(with my library called ‘[JSONWorkers]’) and handle all messages in the
    WebWorkers threads to increase a performance, especially on a mobile
    devices.

-   Use the custom library to create a peer-to-peer(WebRTC) connections without
    the server. How-to-do [demo](https://github.com/cjb/serverless-webrtc)by
    Chris Ball

-   My own serverless technology to create and distribute a video broadcasts
    from one peer to another(like Periscope).

-   The server side based on the oldest nodejs and io.js builds.

-   The server side can be easely distibuted to a several threads (with
    child_process.fork) on a one server or a several servers (by proxying a
    client's requests and [Redis pubsub](https://redis.io/topics/pubsub) as a
    data bus between servers) with my custom developed libraries.

-   Redis is used as a primary storage:

    1.  Redis installation for a user data (accounts) storage use a several
        master nodes, replicated by a slave nodes and watched by the Sentinel.
        It uses the snapshotting on a disk and AOF for the data persistence.

    2.  Redis installations for the current geo information also uses the
        master-slave replication with Sentinel watching on it. All of data is
        storing in memory because of the performance reasons. Also i use the
        techniques to store a geo coordinates as a one numeric hash to reduce a
        memory utilization. This numeric hash have also encoded by cryptographic
        to unable а fake geo coordinates by the users. Server decode the
        received coordinates with the private key to it's origin geo hash.

    3.  My libray called ‘[RedisPipelineCommander]’ backed by
        [ioredis](https://github.com/luin/ioredis) increase the performance of a
        db requests by merging a several requests into a pipeline and send this
        query to the database as a one. It distribute a requests for a several
        redis nodes by a db 'KEY' the query is deal with. When the results are
        obtained from ioredis, the library divides them in the order in which
        they came, handle an errors, and resolves with it a Bluebird Promises,
        stored in the waiting queue.

    4.  Use custom LUA scripts to decrease a number of db calls.

-   There is no registration is needed. Watcha uses OAuth 2.0 for it, backed by
    [hello.js](https://adodson.com/hello.js/) on the client and my custom
    library on the server.

-   For map rendering the [leafletjs](leafletjs.com) with some plugins is used.
    The custom library and the
    [Leaflet.MovingMarker](https://github.com/ewoken/Leaflet.MovingMarker) are
    used to render the user markers and handling of it's position changing..

-   To obtain and watching(for changing in real-time) for the user coordinates
    used a custom library backed by [Geolocation
    API](https://developer.mozilla.org/ru/docs/Web/API/Geolocation).

-   UI is based on [jquerymobile](https://jquerymobile.com) and a several custom
    components.

-   I've created Ubuntu install script for easy installation.

 
-

### My libraries in this project:

-   [RedisPipelineCommander](https://github.com/pashoo2/RedisPipelineCommander/)
    - auto sharding and merge into pipelines of a Redis queries

-   [PeerJS](https://github.com/pashoo2/peerjs) (forked. Origin is
    [here](https://github.com/peers/peerjs)) - Peer.JS with some custom features

-   [JSONWorkers](https://github.com/pashoo2/JSONWorkers)- use WebWorkers to
    parse/stringify JSON
