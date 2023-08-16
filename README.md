# pi4-arm-pmm-client

## These PMM Client tools were build from source code found here

 - https://github.com/percona/pmm-admin
 - https://github.com/percona/pmm-agent
 - https://github.com/percona/node_exporter

## These are not official client tools from Percona

Here are the steps I followed to compile the packages. 

### Compile pmm-admin
```
cd ~/percona/pmm-admin
make release
```

### Compile node_exporter
```
cd ~/percona/node_exporter
make build
```

### Compile pmm-agent
Before we can compile the pmm-agent we need to make a change to the Makefile.

```
cd ~/percona/pmm-agent
vi Makefile
```

Around line 27 you should see:
```
env CGO_ENABLED=1 go build -v -ldflags "-extldflags '-static' $(VERSION_FLAGS)" -tags 'osusergo netgo static_build' -o $(PMM_RELEASE_PATH)/pmm-agent
```

Add GOARCH=arm64 to existing line.
```
env GOARCH=arm64 CGO_ENABLED=1 go build -v -ldflags "-extldflags '-static' $(VERSION_FLAGS)" -tags 'osusergo netgo static_build' -o $(PMM_RELEASE_PATH)/pmm-agent
```

```
make release
```

You can ignore this message:
```
ldd bin/pmm-agent
	not a dynamic executable
make: [Makefile:29: release] Error 1 (ignored)
```

## The compile of tools is complete 

You can find the compiled binary at these locations
```
~/percona/pmm-admin/bin/pmm-admin
~/pmm-agent/bin/pmm-agent
~/node_exporter/node_exporter