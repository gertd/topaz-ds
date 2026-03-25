# Notes

When running with a `remote directory` mode you are delegating all directory calls to the `root directory`. So when that `root` instance is down your directory calls made against the `remote directory` will fail (addres resolve/timeout).

Alternatively you can run topaz in `edge directory` mode in which case the data is synchronized (PULLED) from the `root directory` instance. This is achieved by configuring the `aserto_edge plugin` on the `edge directory` instance like:

    plugins:
      aserto_edge:
        addr: topaz-ds:9292
        enabled: true
        insecure: true
        page_size: 100
        sync_interval: 1
        timeout: 5


Edge synchronization is `pull` and `timer based`, with the shortest time interval being `1m`. The time slot is divided in 4 equal slots, effectively this means that every `15s` the plugin will contact the source (root) directory. 

During the **first** cycle of the synchronization the plugin performs a "mode":"MANIFEST|DIFF" sync, meaning, it ensures the latest manifest is used mode:MANIFEST, and it will perform a full differential sync of the data mode:DIFF. Differential sync will remove items that are not in the source from the target. This is why when running in edge mode you are effectively in READONLY mode, as any changes made on the edge directory, will get lost on the next DIFF sync run. This is why it is best practice to turn off the directory writer service in your configuration.

The **next 3 cycles** are watermark synchronizations "mode":"MANIFEST|WATERMARK" in that mode it only pull the latest changes since the last watermark. The watermark is recorded on the edge in the *.db.sync file, stored in the db directory.


```
cat az2/db/az2.db.sync
 
{"last_updated":"2026-03-25T07:44:32.759460548Z","ts":{"seconds":1774424672,"nanos":759460548},"count":82,"obj_count":33,"rel_count":49}

```

This repo provides a `docker-compose` base demo setup which demonstrates the above.

Simply pull down the repo and run `docker-compose up` will initialize a `root directory` named `ds` a `remote directory`: `az1` and an `edge directory`: `az2` . 

