ako를 통해서 NSX ALB(Formerly AVI)를 통한 K8s를 위한 로드밸런서 서비스를 제공합니다.

ako는 동일 TKG 클러스터 안에서 멀티 AKO 구성을 지원하고 있으며, 기본적으로 최초 구성되는 ako를 primary ture로 구성하고, 추가되는 ako에 대허서는 false 지정을 필요로 합니다.
VKS(vSphere Kubernetes Service - 이전명 TKG 서비스)를 구성할 경우, 기본 수퍼바이저 활성화 시 사용된 NSX ALB를 사용하게 됩니다.
기본 NSX ALB의 경우, 클러스터의 KubeAPI와 통신을 위한 용도로도 사용되기에 사용자 워크로드에 대해서 관리 API 대역과 분리를 위해서는 추가 ako 구성을 하면 됩니다.

ako 배포 시, nameselector label을 지정할 수 있으며, 이 값에 따라서 k8s namespace는 label 설정을 필요로 하며, 이 label을 통한 sync를 맺게 됩니다.
또한, 이러한 ako를 사용한 Service LoadBalancer를 배포하기 위해서는 loadBalancerClass를 함께 Kind: Service에서 type: LoadBalancer와 함께 지정을 해줘야 사용할 수 있습니다.
