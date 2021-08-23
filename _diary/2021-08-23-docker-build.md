```
[+] Building 0.0s (1/2)                                                         
 => [internal] load build definition from .dockerfile                      0.0s
 => => transferring dockerfile: 2B                                         0.0s
failed to solve with frontend dockerfile.v0: failed to read dockerfile: open /var/lib/docker/tmp/buildkit-mount268505054/.dockerfile: no such file or directory
➜  poomgo_backend_plug git:(docker-deploy) ✗ docker build -f Dockerfile -t billing_container . 
[+] Building 96.4s (11/12)                                                      
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 212B                                       0.0s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 34B                                           0.0s
 => [internal] load metadata for docker.io/library/node:12.22-alpine3.12   2.4s
 => [auth] library/node:pull token for registry-1.docker.io                0.0s
 => CACHED [1/7] FROM docker.io/library/node:12.22-alpine3.12@sha256:0257  0.0s
 => [internal] load build context                                          0.1s
 => => transferring context: 95.09kB                                       0.1s
 => [2/7] RUN apk add git                                                  3.6s
 => [3/7] RUN mkdir /app                                                   0.2s
 => [4/7] WORKDIR /app                                                     0.0s 
 => [5/7] COPY package.json .                                              0.0s 
 => ERROR [6/7] RUN npm install                                           90.0s 
------                                                                          
 > [6/7] RUN npm install:                                                       
#11 26.64 npm WARN deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.           
#11 26.65 npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
#11 27.23 npm WARN deprecated uuid@3.3.2: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.
#11 27.98 npm WARN deprecated querystring@0.2.0: The querystring API is considered Legacy. new code should use the URLSearchParams API instead.
#11 34.63 npm WARN deprecated cross-spawn-async@2.2.5: cross-spawn no longer requires a build toolchain, use it instead
#11 34.65 npm WARN deprecated core-js@2.6.12: core-js@<3.3 is no longer maintained and not recommended for usage due to the number of issues. Because of the V8 engine whims, feature detection in old core-js versions could cause a slowdown up to 100x even if nothing is polyfilled. Please, upgrade your dependencies to the actual version of core-js.
#11 36.76 npm WARN deprecated highlight.js@9.18.5: Support has ended for 9.x series. Upgrade to @latest
#11 45.20 npm WARN deprecated har-validator@5.1.5: this library is no longer supported
#11 49.24 npm WARN deprecated sane@4.1.0: some dependency vulnerabilities fixed, support for node < 10 dropped, and newer ECMAScript syntax/features added
#11 61.23 npm WARN deprecated resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
#11 61.23 npm WARN deprecated urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
#11 66.88 npm WARN deprecated axios@0.19.2: Critical security vulnerability fixed in v0.21.1. For more information, see https://github.com/axios/axios/pull/3410
#11 88.62 
#11 88.62 > node-expat@2.4.0 install /app/node_modules/node-expat
#11 88.62 > node-gyp rebuild
#11 88.62 
#11 88.74 gyp ERR! find Python 
#11 88.74 gyp ERR! find Python Python is not set from command line or npm configuration
#11 88.74 gyp ERR! find Python Python is not set from environment variable PYTHON
#11 88.74 gyp ERR! find Python checking if "python" can be used
#11 88.74 gyp ERR! find Python - "python" is not in PATH or produced an error
#11 88.74 gyp ERR! find Python checking if "python2" can be used
#11 88.74 gyp ERR! find Python - "python2" is not in PATH or produced an error
#11 88.74 gyp ERR! find Python checking if "python3" can be used
#11 88.74 gyp ERR! find Python - "python3" is not in PATH or produced an error
#11 88.74 gyp ERR! find Python 
#11 88.74 gyp ERR! find Python **********************************************************
#11 88.74 gyp ERR! find Python You need to install the latest version of Python.
#11 88.74 gyp ERR! find Python Node-gyp should be able to find and use Python. If not,
#11 88.74 gyp ERR! find Python you can try one of the following options:
#11 88.74 gyp ERR! find Python - Use the switch --python="/path/to/pythonexecutable"
#11 88.74 gyp ERR! find Python   (accepted by both node-gyp and npm)
#11 88.74 gyp ERR! find Python - Set the environment variable PYTHON
#11 88.74 gyp ERR! find Python - Set the npm configuration variable python:
#11 88.74 gyp ERR! find Python   npm config set python "/path/to/pythonexecutable"
#11 88.74 gyp ERR! find Python For more information consult the documentation at:
#11 88.74 gyp ERR! find Python https://github.com/nodejs/node-gyp#installation
#11 88.74 gyp ERR! find Python **********************************************************
#11 88.74 gyp ERR! find Python 
#11 88.74 gyp ERR! configure error 
#11 88.74 gyp ERR! stack Error: Could not find any Python installation to use
#11 88.74 gyp ERR! stack     at PythonFinder.fail (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/find-python.js:307:47)
#11 88.74 gyp ERR! stack     at PythonFinder.runChecks (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/find-python.js:136:21)
#11 88.74 gyp ERR! stack     at PythonFinder.<anonymous> (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/find-python.js:179:16)
#11 88.74 gyp ERR! stack     at PythonFinder.execFileCallback (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/find-python.js:271:16)
#11 88.74 gyp ERR! stack     at exithandler (child_process.js:315:5)
#11 88.74 gyp ERR! stack     at ChildProcess.errorhandler (child_process.js:327:5)
#11 88.74 gyp ERR! stack     at ChildProcess.emit (events.js:314:20)
#11 88.74 gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:274:12)
#11 88.74 gyp ERR! stack     at onErrorNT (internal/child_process.js:470:16)
#11 88.74 gyp ERR! stack     at processTicksAndRejections (internal/process/task_queues.js:84:21)
#11 88.74 gyp ERR! System Linux 5.10.25-linuxkit
#11 88.74 gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
#11 88.74 gyp ERR! cwd /app/node_modules/node-expat
#11 88.74 gyp ERR! node -v v12.22.5
#11 88.74 gyp ERR! node-gyp -v v5.1.0
#11 88.74 gyp ERR! not ok 
#11 89.47 npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@~2.3.2 (node_modules/chokidar/node_modules/fsevents):
#11 89.47 npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.3.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})
#11 89.48 npm WARN pdfjs-dist@2.2.228 requires a peer of webpack@^3.0.0 || ^4.0.0-alpha.0 || ^4.0.0 but none is installed. You must install peer dependencies yourself.
#11 89.48 npm WARN pdfjs-dist@2.9.359 requires a peer of worker-loader@^3.0.7 but none is installed. You must install peer dependencies yourself.
#11 89.49 npm WARN worker-loader@2.0.0 requires a peer of webpack@^3.0.0 || ^4.0.0-alpha.0 || ^4.0.0 but none is installed. You must install peer dependencies yourself.
#11 89.50 npm WARN ajv-keywords@3.5.2 requires a peer of ajv@^6.9.1 but none is installed. You must install peer dependencies yourself.
#11 89.50 npm WARN poomgo-backend-plug@1.0.5 No description
#11 89.50 
#11 89.59 npm ERR! code ELIFECYCLE
#11 89.59 npm ERR! errno 1
#11 89.60 npm ERR! node-expat@2.4.0 install: `node-gyp rebuild`
#11 89.60 npm ERR! Exit status 1
#11 89.60 npm ERR! 
#11 89.60 npm ERR! Failed at the node-expat@2.4.0 install script.
#11 89.60 npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
#11 89.62 
#11 89.62 npm ERR! A complete log of this run can be found in:
#11 89.62 npm ERR!     /root/.npm/_logs/2021-08-22T15_10_00_454Z-debug.log
------
executor failed running [/bin/sh -c npm install]: exit code: 1
```

python때문에 안된다
이미지 두개 어떻게 가져오지 ㅜㅜ
