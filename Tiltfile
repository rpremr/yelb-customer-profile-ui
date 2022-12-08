SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='index.docker.io/yelb-customer-profile-ui-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev-space')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')

k8s_custom_deploy(
    'yelb-customer-profile-ui',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes " +
               OUTPUT_TO_NULL_COMMAND +
               " && kubectl get workload yelb-customer-profile-ui --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)


k8s_resource('yelb-customer-profile-ui', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'yelb-customer-profile-ui'}])

allow_k8s_contexts('gke_pa-praghavendran_us-central1_tap13-cluster')
