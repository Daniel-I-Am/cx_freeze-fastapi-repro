# cx_freeze-fastapi-repro
For issue: [#2433](https://github.com/marcelotduarte/cx_Freeze/issues/2433)

Reproducing in Docker for consistency:
```
docker run --rm -it -v "$(pwd):/app" -w /app python:3.12-alpine sh
```

Install with cx_Freeze v7.1.0.post0
```
pip install poetry
poetry install
poetry run python setup.py build_exe
build/exe.linux-x86_64-3.12/hypercorn main --bind 0.0.0.0
```
This fails to start with error message:
```
/app # build/exe.linux-x86_64-3.12/hypercorn main --bind 0.0.0.0
usage: hypercorn [-h] [--access-log ACCESS_LOG] [--access-logfile ACCESS_LOGFILE]
                 [--access-logformat ACCESS_LOGFORMAT] [--backlog BACKLOG]
                 [-b BINDS] [--ca-certs CA_CERTS] [--certfile CERTFILE]
                 [--cert-reqs CERT_REQS] [--ciphers CIPHERS] [-c CONFIG] [--debug]
                 [--error-log ERROR_LOG] [--error-logfile ERROR_LOGFILE]
                 [--graceful-timeout GRACEFUL_TIMEOUT]
                 [--read-timeout READ_TIMEOUT] [--max-requests MAX_REQUESTS]
                 [--max-requests-jitter MAX_REQUESTS_JITTER] [-g GROUP]
                 [-k WORKER_CLASS] [--keep-alive KEEP_ALIVE] [--keyfile KEYFILE]
                 [--keyfile-password KEYFILE_PASSWORD]
                 [--insecure-bind INSECURE_BINDS] [--log-config LOG_CONFIG]
                 [--log-level LOG_LEVEL] [-p PID] [--quic-bind QUIC_BINDS]
                 [--reload] [--root-path ROOT_PATH] [--server-name SERVER_NAMES]
                 [--statsd-host STATSD_HOST] [--statsd-prefix STATSD_PREFIX]
                 [-m UMASK] [-u USER] [--verify-mode VERIFY_MODE]
                 [--websocket-ping-interval WEBSOCKET_PING_INTERVAL] [-w WORKERS]
                 application
hypercorn: error: unrecognized arguments: --multiprocessing-fork pipe_handle=7
```

Install cx_Freeze v7.0.0 and rebuild
```
poetry add cx_freeze=~7.0.0
poetry run python setup.py build_exe
build/exe.linux-x86_64-3.12/hypercorn main --bind 0.0.0.0
```

This runs fine:
```
/app # build/exe.linux-x86_64-3.12/hypercorn main --bind 0.0.0.0
[2024-06-03 10:49:12 +0000] [559] [INFO] Running on http://0.0.0.0:8000 (CTRL + C to quit)
```
