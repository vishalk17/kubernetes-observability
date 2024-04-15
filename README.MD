--------- step 1 ------------
kubectl create ns observability
kubectl apply -f grafana-alert-provisioning.yaml

--------- step 2 ------------ installing promtail 

refer promtail/readme.md

--------- step 3 ------------ installing loki

refer loki/readme.md

--------- step 4 ------------ installing grafana, alertmanager, nodeexporter, prometheus

bash observability-install.sh

--------- step 5 ------------  update alertmanager secrets

- edit and update as per requirement >>  alertmanager.yaml

- cat alertmanager.yaml | base64 -w0

Note : copy this encoded secret

- kubectl get secrets -n observability

- kubectl edit secrets alertmanager-kube-prom-stack-kube-prome-alertmanager -n observability

Note : here you to replace this encoded secret 

- save and exit

----------------------------------

kubectl get svc -n observability

you will get details about following things

grafana ui ( kube-prom-stack-grafana >> 80), 
alertmanager ui (kube-prom-stack-kube-prome-alertmanager > 9093) , 
promtail ui , (promtail-service >> 3101)
prometheus ui >> ( kube-prom-stack-kube-prome-prometheus >> 9090 )



-------------------- done :) ------------

=======================================

how to create alerting rule and apply them for prometheus  ( refer other files in same dir ) ( here we are using alertmanager not the grafana )  ( I have defined rules as per my use case, in ur case it might be different )

create rules file in rules/ dir

then apply using 

kubectl apply -f <rule-file-prometheus.yaml>

===========================================

how to create alerting rule and apply them for loki (logs for container ) : ( here we are using alertmanager not the grafana )  ( I have defined rules as per my use case, in ur case it might be different )

cd loki

edit or update or add new rules >>    loki-rules-configmap.yaml 

kubectl apply -f loki-rules-configmap.yaml 

======================================