SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='registry.harbor.t3u.link/tap-project/spring-sensors-rabbit-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
   'spring-sensors-rabbit-dev',
   apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
       " --local-path " + LOCAL_PATH +
       " --source-image " + SOURCE_IMAGE +
       " --namespace " + NAMESPACE +
       " --yes >/dev/null" +
       " && kubectl get workload spring-sensors-rabbit-dev --namespace " + NAMESPACE + " -o yaml",
   delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes" ,
   deps=['pom.xml', './target/classes'],
   container_selector='workload',
   live_update=[
       sync('./target/classes', '/workspace/BOOT-INF/classes')
   ]
)

k8s_resource('spring-sensors-rabbit-dev', port_forwards=["8080:8080"],
   extra_pod_selectors=[{'serving.knative.dev/service': 'spring-sensors-rabbit-dev'}])
allow_k8s_contexts('cloudgate@pvanderbleek@tap-dev.eu-central-1.eksctl.io')