# Ansible Role: docker_deployment

Deploy your docker images.

[https://www.docker.com/](https://www.docker.com/)

# Role Variables

```
registry_type: aws-ecr
```

The registry type.

Posible values:
 * aws-ecr
 * self-hosted (like nexus docker registry or docker offical registry)
 * ~~dockerhub~~ (unsupported)
 * archive-tar (docker save tar file)

```
registry_region: us-west-1
```

Region of AWS ECR, only required in registry_type=aws-ecr

```
aws_access_key_id: "{{ lookup('env', 'AWS_ACCESS_KEY_ID') }}"
```

Access-Key-Id of AWS IAM, please set an environment `AWS_ACCESS_KEY_ID`, only required in registry_type=aws-ecr

```
aws_secret_access_key: "{{ lookup('env', 'AWS_SECRET_ACCESS_KEY') }}"
```

Secret-Access-Key of AWS IAM, please set an environment `AWS_SECRET_ACCESS_KEY`, only required in registry_type=aws-ecr

```
registry_username: "{{ lookup('env', 'REGISTRY_USERNAME') }}"
```

Username of registry, please set an environment `REGISTRY_USERNAME`, only required in registry_type=self-hosted

```
registry_password: "{{ lookup('env', 'REGISTRY_PASSWORD') }}"
```

Password of registry, please set an environment `REGISTRY_PASSWORD`, only required in registry_type=self-hosted

```
archive_username: "{{ lookup('env', 'ARCHIVE_USERNAME') }}"
```

Username of archive url, please set an environment `ARCHIVE_USERNAME`, only required in archive_type=archive-tar

```
archive_password: "{{ lookup('env', 'ARCHIVE_PASSWORD') }}"
```

Password of archive url, please set an environment `ARCHIVE_PASSWORD`, only required in registry_type=archive-tar

```
archive_url: "{{ lookip('env', 'ARCHIVE_URL') }}"
```

Url of archive server, without archive tar.gz file name, please set an environment `ARCHIVE_URL`, only required in registry_type=archive-tar

```
registry_scheme: https
```

Scheme type of registry

Posible values:
 * https (default)
 * http

```
registry_url: dockerhub.com
```

Docker registry url

```
image_team: moa
```

Team name of the image owned.

```
image_project: caddy
```

Project name of the image.

```
image_tag: latest
```

Tag of image to deploy

```
container_name: caddy-server-1
```

Name of the container

```
container_ports: []
```

Port to expose

```
container_restart_policy: always
```

Restart policy of container

```
container_environment: {}
```

Environment of container, dictionary of key,value pairs.

```
container_volumes: []
```

Mounted volumes of container

```
container_extra_hosts:
```

Add hostname mappings. Use the same values as the docker client --add-host parameter.

```
container_command: ''
```

Command to run

Sees: (https://docs.docker.com/compose/compose-file/#command)[https://docs.docker.com/compose/compose-file/#command]

```
container_privileged: no
```

Give extended privileges to this container

```
container_dns_servers: []
```

List of custom DNS servers.

```
container_dns_search_domains: []
```

List of custom DNS search domains.

# Example Playbook

```
- hosts: server
  vars:
    registry_url: dockerhub.com
    image_team: moa
    image_project: caddy
    image_tag: latest
    container_name: caddy-server-01
    container_ports:
      - 80:80
      - 443:443
    container_restart_policy: always
    container_environment:
      HOST_ID: 100
    container_volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:ro
      - ${PWD}/data:/data
    container_privileged: no
  roles:
    - { role: honomoa.docker_deployment }
```

# License
CC BY-SA 3.0
