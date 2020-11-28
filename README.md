# Forked from Andrey <MrEcco> Burindin's repo, Please visit below URL for better explaination for each component of this complete and very neat Airflow-Kubernetes-IDE:
https://mrecco.medium.com/apache-airflow-kubernetes-on-local-how-to-simplify-development-for-sophisticated-platform-87078b8129d9


# My Objective is to learn Airflow on kubernetes as well as implement data pipelines for ML. My Changes so far are as follows:

1) Updated cluster.yml to change Kind node image from kindest/node:v1.16.4 to kindest/node:v1.17.0 , added -- image flag in routines.py create  
2) Added @localkind in order to enable working offline on your local airflow environments, However kubernetes - dashboard still not work offline
3) Bind Port 5432 for accessing postgres from local client
4) Added PYTHONPATH , extra r/w mount for scripts in podtemplate.yml so that custom modules can be imported from scripts folder 
5) Added few more dags for training and prediction , updated requirements.txt accordingly

# Tips: 

1) Kubernetes dashboard :: http://127.0.0.1:8008/
2) In order to get Kube lens to work, I did following:
    a) sudo hostnamectl set-hostname kubernetes
    b) sudo gedit /etc/hosts --> added 127.0.0.53	kubernetes
    c) hostnamectl --for verification
3) In case, Volume and Overlay2 directories of docker gets huge then do folowing (only in dev):
    a) docker system df --to check
    b) docker volume prune  (for dangling volumes, this will hard reset your meta databases)
    c) docker system prune -a  (After executing make start, Only if above do not reduces much)
    d) docker system prune -a  (After executing make stop, Only if above do not reduces much) 
4) Some other docker maintainence:    
    a) docker image prune (In case you want to remove dangling (un tagged) images)
    b) docker image prune -a --filter "until=48h" (In acasecse you want to remove old images)


# Apache Airflow IDE

Apache Airflow IDE with Kubernetes Operator capabilities.

Kubernetes can be painful for any untrained specialist. This is preconfigured local
development environment with full power of Kubernetes Operator, but with whole
bootstrapment automations. This repository just provide easy way to use Apache Airflow
and real Kubernetes cluster on the local. Dont use it in production! Deployment ready
to use by analists with python stack which have no any understanding about kubernetes :)

## Meet the Airflow

This is easy to welcome and almost full description about capabilities provided
by Airflow. Recommended to read before any action.

https://airflow.apache.org/docs/stable/concepts.html

## IDE

This is local bootstrapment of Apache Airflow service! It have fully-featured service
with API, web UI, scheduler, database server (required for service components), small
kubernetes cluster and some helpers which automate cluster routines.

Components:

- Web interface. Doesnt handle AAA-features, mean have no authorization.

- Scheduler. Defaulted to spawn workloads to kubernetes.

- Kubernetes. Simplified version, but fully-featured. Based on kubernetes-in-docker
(see [kind project](https://kind.sigs.k8s.io/)) tehnique (but all wrapped by docker-in-docker).

- Docker registry. Helps to use local builded airflow image to kubernetes-accessible
place.

## How to use

All what you need in Makefile. And you must neet to install Makefile resolver:
`brew install make` on MacOS or `apt-get install make || yum install make`
on Linux. This is required single time only. Actualy, this works on Windows, but
this manual doesnt cover how to install `make` into Windows.

For start your system, run this:

```bash
make start
```

After few-five minutes your system is ready to use: see `http://127.0.0.1:8080`
for Apache Airflow web interface. If you need, you can look for kubernetes
web interface: `http://127.0.0.1:8008`.

You can rebuild images for add supported packages to Apache Airflow. Just add
required libs to `docker/airflow/requirements.txt` file and start building
process:

```bash
# add libs to docker/airflow/requirements.txt
make rebuild
make restart
```

For stop (and release used resources), run this:

```bash
make stop
```

## Links

Airflow Docs: https://airflow.apache.org/docs

Kubernetes API Reference: https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/

## License

Licensed by MIT.
