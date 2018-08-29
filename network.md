# Network


## Wait for a port to be available 
```bash
while [ "$(bash -c "echo -n > /dev/tcp/localhost/80" 2> /dev/null; echo $?)" -eq 1 ]; do sleep 1; done; echo "Port available"
```
```bash
while ! bash -c "echo > /dev/tcp/localhost/80" 2> /dev/null; do sleep 1; done; echo "Port available"
```

#### Notes
* _2> /dev/null_ silences errors (most likely 'connection refused')
* '$?' reads the latest exit code (produced by _bash -c_ in this case)
* When running this inside another _bash -c_ (e.g. docker entrypoint), all '$' signs have to be escaped by prefixing another '$' sign (resulting in '$$')
* You might want to add _timeout <duration>_

#### Links
* https://www.gnu.org/software/bash/manual/html_node/Redirections.html#Redirections
* https://unix.stackexchange.com/questions/5277/how-do-i-tell-a-script-to-wait-for-a-process-to-start-accepting-requests-on-a-po


