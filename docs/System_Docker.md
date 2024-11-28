### Main

[C2][Main](index.md)😃.

### IMAGE
[sg](https://hub.docker.com/r/sourcegraph/server)
- download
`sudo docker pull sourcegraph/server:3.37.0`

- list 
`docker image ls`

- remove
`docker image rm sourcegraph/server:3.37.0`



### INFO
`sudo docker info`

### RUN

- in front
```
sudo docker run --publish 7080:7080 --publish 127.0.0.1:3370:3370 --rm --volume ~/.sourcegraph/config:/etc/sourcegraph --volume ~/.sourcegraph/data:/var/opt/sourcegraph sourcegraph/server:3.37.0
Ctrl + C
```

- advance
```
-d
--name sg
```

### STOP
`sudo docker stop`

### STAT
```
sudo docker ps -a
sudo docker history  sourcegraph/server:3.37.0

```

### Gitlab
```
sudo docker pull gitlab/gitlab-ce

sudo docker run \
  --hostname gitlab.example.com \
  --publish 8443:443 --publish 8081:80 -p 2222:22 \
  --name gitlab \
  --restart always \
  --volume $HOME/_docker/gitlab/config:/etc/gitlab \
  --volume $HOME/_docker/gitlab/logs:/var/log/gitlab \
  --volume $HOME/_docker/gitlab/data:/var/opt/gitlab \
  -v /etc/localtime:/etc/localtime \
  -d \
  gitlab/gitlab-ce:latest
```

<a href="#top">Back to top</a>
