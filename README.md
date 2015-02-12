# docker.ottemo.io
Docker Ottemo.io

1. Pull image CentOS latest from Docker Hub:
    docker pull centos:latest

2. Build foundation container:
    cd foundaton & docker build -t ottemo/foundation .

3. Run ottemo/foundation container in privileged mode:
    docker run --privileged -ti -v /sys/fs/cgroup:/sys/fs/cgroup:ro ottemo/foundation
