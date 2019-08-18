## Setting Up Docker

Currently I am using a typical windows 7 machine. With my previous experiance i decided not to use  docker for windows. Instead use Ubuntu inside Virtual Box and install docker in ubuntu guest machine.

Now to connect your host machine to docker daemon, you need to make docker daemon listen on a tcp port. (default port : 2375)

### Running Docker as daemon

To configure/ customize daemon setting, you need to create a `daemon.json` file in `/etc/docker/` directory.
a minimal example is here :
    
     {
    "debug": false,
    "tls": false,
    "log-level": "info",
    "hosts": ["tcp://0.0.0.0:2375"]
    }

now restart the docker service : `systemctl stop docker` and `systemctl start docker`

you might get some error as below 
   
    root@biplab-VirtualBox:/etc/docker# systemctl start docker
    Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.

reason : 
    
     root@biplab-VirtualBox:/etc/docker# systemctl status docker.service
    ● docker.service - Docker Application Container Engine
    Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
    Active: failed (Result: exit-code) since Sat 2017-10-21 05:32:44 UTC; 17s ago
     Docs: https://docs.docker.com
    Process: 2982 ExecStart=/usr/bin/dockerd -H fd:// (code=exited, status=1/FAILURE)
    Main PID: 2982 (code=exited, status=1/FAILURE)
    Oct 21 05:32:44 biplab-VirtualBox systemd[1]: Starting Docker Application Container Engine...
    Oct 21 05:32:44 biplab-VirtualBox dockerd[2982]: unable to configure the Docker daemon with file /etc/docker/daemon.json: the following directives are specified both as a flag and in the configuration f
    Oct 21 05:32:44 biplab-VirtualBox systemd[1]: docker.service: Main process exited, code=exited, status=1/FAILURE
    Oct 21 05:32:44 biplab-VirtualBox systemd[1]: Failed to start Docker Application Container Engine.
    Oct 21 05:32:44 biplab-VirtualBox systemd[1]: docker.service: Unit entered failed state.
    Oct 21 05:32:44 biplab-VirtualBox systemd[1]: docker.service: Failed with result 'exit-code'.


to avoid this edit ` /lib/systemd/system/docker.service` file

      [Unit]
      Description=Docker Application Container Engine
      Documentation=https://docs.docker.com
      After=network.target docker.socket firewalld.service
      Requires=docker.socket
      
      [Service]
      Type=notify
      # the default is not to use systemd for cgroups because the delegate issues still
      # exists and systemd currently does not support the cgroup feature set required
      # for containers run by docker
      ExecStart=/usr/bin/dockerd -H fd://
      ExecReload=/bin/kill -s HUP $MAINPID
      LimitNOFILE=1048576
      # Having non-zero Limit*s causes performance problems due to accounting overhead
      # in the kernel. We recommend using cgroups to do container-local accounting.
      LimitNPROC=infinity
      LimitCORE=infinity
      # Uncomment TasksMax if your systemd version supports it.
      # Only systemd 226 and above support this version.
      TasksMax=infinity
      TimeoutStartSec=0
      # set delegate yes so that systemd does not reset the cgroups of docker containers
      Delegate=yes
      # kill only the docker process, not all processes in the cgroup
      KillMode=process
      
      [Install]
      WantedBy=multi-user.target

and delete `-H fd://` from line number 13.
and run `systemctl daemon-reload` and `systemctl start docker` 

Now you can see `dockerd` listening on port `2375`

    root@biplab-VirtualBox:/etc/docker# netstat -npl
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
    tcp6       0      0 :::2375                 :::*                    LISTEN      3166/dockerd

Now bind the guest port `2375` to any of your host machine port (preferably 2375). Go to Virtual Box Manager. select the running VM.
Settings -> Network -> Advanced -> Port Forwaring
create a new rule.

### why you might need docker daemon to listen on a tcp port ?

 * you may want to deploy your application to a running docker daemon remotly via any plugin (eg. docker maven plugin) 