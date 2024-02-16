LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
WAIT_TIMEOUT = os.getenv("WAIT_TIMEOUT", default='10m00s')
TYPE = os.getenv("TYPE", default='web')
NAME = "csharp-weatherforecast"

k8s_custom_deploy(
  NAME,
  apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
            " --local-path " + LOCAL_PATH +
            " --build-env BP_DEBUG_ENABLED=true" +
            " --namespace " + NAMESPACE +
            " --wait-timeout " + WAIT_TIMEOUT +
            " --type " + TYPE +
            " --yes " +
            " -oyaml",
  delete_cmd="tanzu apps workload delete " + NAME + " --namespace " + NAMESPACE + " --yes",
  deps=['./bin'],
  container_selector='workload',
  live_update=[
    sync('./bin/Debug/net6.0', '/workspace')
  ]
)

k8s_resource(NAME, port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'csharp-weatherforecast', 'app.kubernetes.io/component': 'run'}])

allow_k8s_contexts('<context>') # update the context
