# notebookops

Materials to go along with my JupyterCon 2020 talk: [NotebookOps: A pattern for building notebook-centric data platforms](https://cfp.jupytercon.com/2020/schedule/presentation/163/notebookops-a-pattern-for-building-notebook-centric-data-platforms/).

You can read about how to set up [JupyterHub](https://github.com/jupyterhub/jupyterhub) and [Airflow](https://github.com/apache/airflow) on [microk8s](https://microk8s.io/) in [this blog post](https://vinayak.io/2020/09/10/day-24-jupyterhub-airflow-microk8s/).

## Installation

### JupyterHub

```
$ kubectl create namespace jhub
$ RELEASE=jhub NAMESPACE=jhub \
    microk8s helm3 upgrade --cleanup-on-fail \
    --install $RELEASE jupyterhub/jupyterhub \
    --namespace $NAMESPACE \
    --version=0.9.0 \
    --values jupyterhub/values.yml
```

### Airflow

```
$ kubectl create namespace airflow
$ RELEASE=airflow NAMESPACE=airflow \
    microk8s helm3 upgrade --cleanup-on-fail \
    --install $RELEASE stable/airflow \
    --namespace $NAMESPACE \
    --version=7.7.0 \
    --values airflow/values.yml
```

## airflow-dag

You can use something like [`airflow-dag`](https://github.com/vinayak-mehta/airflow-dag) in your CI workflow to build notebook dags created on JupyterHub and push them to Airflow's dags folder as described in the talk.

```
$ airflow-dag build -t airflow/templates -c airflow/configs/dag.yml -o airflow/dags
```

I'll also add a GitHub action that does that and pushes new dags to a GitHub repo where they're maintained, as an example.
