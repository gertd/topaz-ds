# Annotated Log Output

### Startup service initialization

```
> docker-compose up

[+] up 4/4
 ✔ Image ghcr.io/aserto-dev/topaz:latest Pulled                                                                                                                                                                             1.8s
 ✔ Container topaz-ds                    Created                                                                                                                                                                            0.0s
 ✔ Container topaz-az1                   Created                                                                                                                                                                            0.0s
 ✔ Container topaz-az2                   Created                                                                                                                                                                            0.0s
Attaching to topaz-az1, topaz-az2, topaz-ds
topaz-ds  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:46Z","message":"Starting 0.0.0.0:9494 health server"}
topaz-ds  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:46Z","message":"Starting 0.0.0.0:9696 metric server"}
topaz-ds  | {"level":"info","component":"edge-ds","component":"kvs","db_path":"/db/ds.db","time":"2026-03-25T07:43:46Z","message":"open"}
topaz-ds  | {"level":"info","component":"edge-ds","current":"0.0.9","time":"2026-03-25T07:43:46Z","message":"schema_version"}
topaz-ds  | {"level":"info","component":"edge-ds","component":"kvs","db_path":"/db/ds.db","time":"2026-03-25T07:43:46Z","message":"close"}
topaz-ds  | {"level":"info","component":"edge-ds","component":"directory","component":"kvs","db_path":"/db/ds.db","time":"2026-03-25T07:43:46Z","message":"open"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:46Z","message":"Starting 0.0.0.0:9494 health server"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:46Z","message":"Starting 0.0.0.0:9696 metric server"}
topaz-az1  | {"level":"info","time":"2026-03-25T07:43:46Z","message":"model service not configured, you will not be able to read or update the directory manifest"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:46Z","message":"Starting 0.0.0.0:9494 health server"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:46Z","message":"Starting 0.0.0.0:9696 metric server"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"kvs","db_path":"/db/az2.db","time":"2026-03-25T07:43:46Z","message":"open"}
topaz-az2  | {"level":"info","component":"edge-ds","current":"0.0.9","time":"2026-03-25T07:43:46Z","message":"schema_version"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"kvs","db_path":"/db/az2.db","time":"2026-03-25T07:43:46Z","message":"close"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","component":"kvs","db_path":"/db/az2.db","time":"2026-03-25T07:43:46Z","message":"open"}
```

### Edge initialization

First edge sync run, note that the health endpoint reports the `sync` service status as `NOT_SERVING` until the initialization is finished. This can be used to detect if the `edge directory` is READY to be used.

```
topaz-az2  | {"level":"info","component":"edge.plugin","id":"-","enabled":true,"interval":1,"time":"2026-03-25T07:43:46Z","message":"EdgePlugin.Start"}
topaz-az2  | {"level":"info","component":"edge.plugin","dispatch":"2026-03-25T07:43:47Z","time":"2026-03-25T07:43:47Z","message":"scheduler"}
topaz-az2  | {"level":"info","component":"edge.plugin","status":"started","time":"2026-03-25T07:43:47Z","message":"sync-task"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","mode":"MANIFEST|DIFF","time":"2026-03-25T07:43:47Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","mode":"MANIFEST","time":"2026-03-25T07:43:47Z","message":"manifest"}
topaz-ds   | {"level":"info","component":"edge.plugin","service":"sync","status":"NOT_SERVING","time":"2026-03-25T07:43:47Z","message":"health"}
topaz-ds   | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:47Z","message":"Starting 0.0.0.0:9292 gRPC server"}
topaz-ds   | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:47Z","message":"Starting 0.0.0.0:9393 gateway server"}
topaz-az1  | {"level":"info","component":"edge.plugin","service":"sync","status":"NOT_SERVING","time":"2026-03-25T07:43:47Z","message":"health"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:47Z","message":"Starting 0.0.0.0:9292 gRPC server"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:47Z","message":"Starting 0.0.0.0:9393 gateway server"}
topaz-az2  | {"level":"info","component":"edge.plugin","service":"sync","status":"NOT_SERVING","time":"2026-03-25T07:43:47Z","message":"health"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:47Z","message":"Starting 0.0.0.0:9292 gRPC server"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:43:47Z","message":"Starting 0.0.0.0:9393 gateway server"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","mode":"DIFF","time":"2026-03-25T07:43:47Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:43:47Z","message":"producer"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:43:47Z","message":"subscriber"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","received":82,"objects":33,"relations":49,"time":"2026-03-25T07:43:47Z","message":"producer"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","received":82,"objects":33,"relations":49,"errors":0,"time":"2026-03-25T07:43:47Z","message":"subscriber"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:43:47Z","message":"difference"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","delete_objects":0,"deleted_relations":0,"errors":0,"time":"2026-03-25T07:43:47Z","message":"difference"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","duration":"28.386917ms","time":"2026-03-25T07:43:47Z","message":"sync-run"}
```

### Health `sync` service status updated to `SERVING` after the first `MANIFEST|DIFF` sync was completed. 

```
topaz-az2  | {"level":"info","component":"edge.plugin","service":"sync","status":"SERVING","time":"2026-03-25T07:43:47Z","message":"health"}
topaz-az2  | {"level":"info","component":"edge.plugin","status":"finished","time":"2026-03-25T07:43:47Z","message":"sync-task"}
```

### First `WATERMARK|MANIFEST` sync step (2/4)

```
topaz-az2  | {"level":"info","component":"edge.plugin","interval":"15s","next-run":"2026-03-25T07:44:02Z","time":"2026-03-25T07:43:47Z","message":"scheduler"}
topaz-az2  | {"level":"info","component":"edge.plugin","dispatch":"2026-03-25T07:44:02Z","time":"2026-03-25T07:44:02Z","message":"scheduler"}
topaz-az2  | {"level":"info","component":"edge.plugin","status":"started","time":"2026-03-25T07:44:02Z","message":"sync-task"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","mode":"WATERMARK|MANIFEST","time":"2026-03-25T07:44:02Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","mode":"MANIFEST","time":"2026-03-25T07:44:02Z","message":"manifest"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","mode":"WATERMARK","time":"2026-03-25T07:44:02Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:44:02Z","message":"producer"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:44:02Z","message":"subscriber"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","received":82,"objects":33,"relations":49,"time":"2026-03-25T07:44:02Z","message":"producer"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","received":82,"objects":33,"relations":49,"errors":0,"time":"2026-03-25T07:44:02Z","message":"subscriber"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","duration":"14.949458ms","time":"2026-03-25T07:44:02Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge.plugin","status":"finished","time":"2026-03-25T07:44:02Z","message":"sync-task"}
topaz-az2  | {"level":"info","component":"edge.plugin","interval":"15s","next-run":"2026-03-25T07:44:17Z","time":"2026-03-25T07:44:02Z","message":"scheduler"}
```

### Second `WATERMARK|MANIFEST` sync step (3/4)

```
topaz-az2  | {"level":"info","component":"edge.plugin","dispatch":"2026-03-25T07:44:17Z","time":"2026-03-25T07:44:17Z","message":"scheduler"}
topaz-az2  | {"level":"info","component":"edge.plugin","status":"started","time":"2026-03-25T07:44:17Z","message":"sync-task"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","mode":"MANIFEST|WATERMARK","time":"2026-03-25T07:44:17Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","mode":"MANIFEST","time":"2026-03-25T07:44:17Z","message":"manifest"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","mode":"WATERMARK","time":"2026-03-25T07:44:17Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:44:17Z","message":"producer"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:44:17Z","message":"subscriber"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","received":82,"objects":33,"relations":49,"time":"2026-03-25T07:44:17Z","message":"producer"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","received":82,"objects":33,"relations":49,"errors":0,"time":"2026-03-25T07:44:17Z","message":"subscriber"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","duration":"15.585208ms","time":"2026-03-25T07:44:17Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge.plugin","status":"finished","time":"2026-03-25T07:44:17Z","message":"sync-task"}
topaz-az2  | {"level":"info","component":"edge.plugin","interval":"15s","next-run":"2026-03-25T07:44:32Z","time":"2026-03-25T07:44:17Z","message":"scheduler"}
```

### Third `WATERMARK|MANIFEST` sync step (4/4)

```
topaz-az2  | {"level":"info","component":"edge.plugin","dispatch":"2026-03-25T07:44:32Z","time":"2026-03-25T07:44:32Z","message":"scheduler"}
topaz-az2  | {"level":"info","component":"edge.plugin","status":"started","time":"2026-03-25T07:44:32Z","message":"sync-task"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","mode":"WATERMARK|MANIFEST","time":"2026-03-25T07:44:32Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","mode":"MANIFEST","time":"2026-03-25T07:44:32Z","message":"manifest"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","mode":"WATERMARK","time":"2026-03-25T07:44:32Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:44:32Z","message":"producer"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"started","time":"2026-03-25T07:44:32Z","message":"subscriber"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","received":82,"objects":33,"relations":49,"time":"2026-03-25T07:44:32Z","message":"producer"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","received":82,"objects":33,"relations":49,"errors":0,"time":"2026-03-25T07:44:32Z","message":"subscriber"}
topaz-az2  | {"level":"info","component":"edge-ds","component":"directory","status":"finished","duration":"13.937209ms","time":"2026-03-25T07:44:32Z","message":"sync-run"}
topaz-az2  | {"level":"info","component":"edge.plugin","status":"finished","time":"2026-03-25T07:44:32Z","message":"sync-task"}
topaz-az2  | {"level":"info","component":"edge.plugin","interval":"15s","next-run":"2026-03-25T07:44:47Z","time":"2026-03-25T07:44:32Z","message":"scheduler"}
```

### Stopping `docker-compose` (Ctrl+C)


```
Gracefully Stopping... press Ctrl+C again to force
Container topaz-az1 Stopping
Container topaz-az2 Stopping
topaz-az2  | {"level":"info","component":"edge.plugin","id":"-","enabled":true,"interval":1,"time":"2026-03-25T07:44:44Z","message":"EdgePlugin.Stop"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9494 health server"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9494 health server"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9696 metric server"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9393 gateway server"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9292 gRPC server"}
topaz-az2  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Running cleanups for 0.0.0.0:9292"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9696 metric server"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9393 gateway server"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9292 gRPC server"}
topaz-az1  | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Running cleanups for 0.0.0.0:9292"}
Container topaz-az2 Stopped
topaz-az2 exited with code 0
Container topaz-az1 Stopped
Container topaz-ds Stopping
topaz-az1 exited with code 0
topaz-ds   | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9494 health server"}
topaz-ds   | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9696 metric server"}
topaz-ds   | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9393 gateway server"}
topaz-ds   | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Stopping 0.0.0.0:9292 gRPC server"}
topaz-ds   | {"level":"info","component":"service-manager","time":"2026-03-25T07:44:44Z","message":"Running cleanups for 0.0.0.0:9292"}
Container topaz-ds Stopped
topaz-ds exited with code 0
```