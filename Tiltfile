SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.nvspdren.navy.mil/tap/workloads/gary-java-web-app-gary-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='gary')
k8s_custom_deploy(
    'gary-java-web-app-gary-source',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes " +
               "--registry-ca-cert C:\\workspace\\sts-workspace\\tanzu\\application-accelerator-samples\\tanzu-java-web-app\\cert\\harbor.nvspdren.navy.mil.crt --registry-username admin --registry-password Harbor12345" +
               " && kubectl get workload gary-java-web-app-gary-source --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('gary-java-web-app-gary-source', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'gary-java-web-app-gary-source', 'app.kubernetes.io/component': 'run'}])

allow_k8s_contexts('tap')
