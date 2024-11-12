# ako
VDS with AVI 환경의 vSphere IaaS Control Plane의 Supervisor에 구성된 TKG Service 클러스터에 대한 Default LB가 아닌 추가 LB를 구성 합니다.
타사 LB를 구성할수도 있으며, 동일한 AVI LB에 대해서, 추가 VIP-Frontend-Network를 제공하여, 구성할 수 있습니다.

Values.yaml 다운로드하여, AVI controller IP 및 Account 정보를 변경합니다.
AVI Controller에서 설정한 IPAM에 포함된 VIP Network로 정보를 변경합니다.
Cloud type 및 SE Group type을 변경합니다.
L7 Only 혹은 L4와 함께 사용할 것인지에 대한 옵션 값을 조정합니다. (True: Only L7, False: L4/L7)
L7 Ingress 사용 여부를 선택합니다.

helm install --generate-name oci://projects.registry.vmware.com/ako/helm-charts/ako --version 1.11.4 -f ./values.yaml  --set ControllerSettings.controllerHost=172.18.10.192 --set avicredentials.username='admin' --set avicredentials.password='VMware1!' --set AKOSettings.primaryInstance=true --namespace=avi-system

Air-gapped 환경에서는 ako image를 사전에 정의된 private registry에 업로드한 후, helm 명령문에 이미지 경로를 변경합니다.

Application을 생성하고, Service LoadBalancer를 추가 합니다.

service.yaml
 -----
spec:
  type: LoadBalancer
  loadBalancerClass: ako.vmware.com/avi-lb

loadBalancerClass를 배포된 avi-lb로 지정하여, Default LB로부터 external IP를 공급받지 않으며, ako를 통해서 구성된 IPAM으로부터 external IP를 공급받습니다.

Supervisor 활성화 시, 배포된 AVI를 KubeAPI 서버 통신용으로 사용하고, TKG Service workload cluster에 대해서는 독립적인 VIP Network를 구성할 수 있습니다.
동일한 NSX ALB(AVI) controller 사용해서 구성이 가능합니다.
