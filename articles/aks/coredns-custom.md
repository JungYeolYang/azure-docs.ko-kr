---
title: CoreDNS AKS (Azure Kubernetes Service)에 대 한 사용자 지정
description: 하위 도메인을 추가 하 여 Azure Kubernetes Service (AKS)를 사용 하 여 사용자 지정 DNS 끝점 확장 CoreDNS 사용자 지정 하는 방법 알아보기
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 03/15/2019
ms.author: jnoller
ms.openlocfilehash: 9186c5ff7c6fbc68487a1ccff0fc1d2d1478df79
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466453"
---
# <a name="customize-coredns-with-azure-kubernetes-service"></a>Azure Kubernetes Service를 사용 하 여 CoreDNS 사용자 지정

Azure Kubernetes Service (AKS)를 사용 합니다 [CoreDNS] [ coredns] 클러스터 DNS 관리 및 모든 해상도 대 한 프로젝트 *1.12.x* 및 더 높은 클러스터. 이전에 kube-dns 프로젝트 사용 되었습니다. 이 kube-dns 프로젝트는 이제 사용 되지 않습니다. CoreDNS 사용자 지정 및 Kubernetes에 대 한 자세한 내용은 참조는 [업스트림 공식 설명서][corednsk8s]합니다.

AKS 관리 되는 서비스 이므로 CoreDNS에 대 한 기본 구성을 수정할 수 없습니다 (한 *CoreFile*). Kubernetes를 사용 하는 대신 *ConfigMap* 기본 설정을 재정의할 수 있습니다. AKS CoreDNS ConfigMaps 기본값을 보려면 사용 하 여는 `kubectl get configmaps coredns -o yaml` 명령입니다.

이 문서에서는 AKS에서 CoreDNS의 기본 사용자 지정 옵션에 대 한 ConfigMaps를 사용 하는 방법을 보여 줍니다.

> [!NOTE]
> `kube-dns` 제공 되는 다른 [사용자 지정 옵션] [ kubednsblog] Kubernetes 구성 맵을 통해. CoreDNS 됩니다 **되지** kube-dns를 사용 하 여 이전 버전과 호환입니다. 이전에 사용한 사용자 지정 CoreDNS 사용에 대 한 업데이트 되어야 합니다.

## <a name="before-you-begin"></a>시작하기 전에

이 문서에서는 기존 AKS 클러스터가 있다고 가정합니다. AKS 클러스터에 필요한 경우 참조 [Azure CLI를 사용 하 여] AKS 빠른 시작 [aks-빠른 시작-cli] [Azure portal을 사용 하 여] 또는 [aks 빠른 시작-포털].

## <a name="change-the-dns-ttl"></a>DNS TTL을 변경

CoreDNS에서 구성 하려는 경우 시나리오 중 하나를 낮추거나 DNS 이름을 캐시 하는 것에 대 한 TTL (Live) 설정에 시간을 발생 시킵니다. 이 예제에서는 TTL 값을 변경 하겠습니다. 기본적으로이 값은 30 초입니다. DNS 캐시 옵션에 대 한 자세한 내용은 참조는 [공식 CoreDNS docs][dnscache]합니다.

다음 예제 ConfigMap을 `name` 값입니다. 기본적으로 자체 CoreFile를 수정 하는 것이 유형의 사용자 지정 CoreDNS에서는. AKS를 사용 하는 *coredns-사용자 지정* ConfigMap 자신만 구성을 통합 하 고 주 CoreFile 후 로드 됩니다.

다음 예제에서는 CoreDNS 하는 모든 도메인에 대 한 (으로 표시 합니다 `.` 에서 `.:53`), 53 (기본 DNS 포트) 포트, 캐시 TTL 15로 설정 (`cache 15`). 라는 파일을 만들고 `coredns-custom.json` 다음 예제에서는 구성을 붙여 넣습니다.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom # this is the name of the configmap you can overwrite with your changes
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    .:53 {
        cache 15  # this is our new cache value
    }
```

사용 하 여 ConfigMap 만듭니다는 [kubectl 적용 configmap] [ kubectl-apply] 명령 및 YAML 매니페스트의 이름을 지정:

```console
kubectl apply configmap coredns-custom.json
```

사용자 지정 항목 적용 되었는지 확인 하려면 사용 합니다 [kubectl get configmaps] [ kubectl-get] 지정에 *coredns-사용자 지정* ConfigMap:

```
kubectl get configmaps coredns-custom -o yaml
```

이제 CoreDNS ConfigMap을 다시 로드를 강제 합니다. [kubectl 삭제 pod] [ kubectl delete] 명령을 삭제 되지 않으며 가동 중지 시간이 발생 하지 않습니다. `kube-dns` pod 삭제 되 고 Kubernetes 스케줄러에서 다음 다시 만듭니다. 이러한 새 pod 변경 된 TTL 값을 포함합니다.

```console
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="rewrite-dns"></a>Rewrite DNS

해야 하는 한 가지 시나리오 즉석에서 DNS 이름을 다시 쓰기를 수행 하는 것입니다. 다음 예제에서는 대체 `<domain to be written>` 사용 하는 정규화 된 도메인 이름입니다. 라는 파일을 만들고 `coredns-custom.json` 다음 예제에서는 구성을 붙여 넣습니다.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    <domain to be rewritten>.com:53 {
        errors
        cache 30
        rewrite name substring <domain to be rewritten>.com default.svc.cluster.local
        proxy .  /etc/resolv.conf # you can redirect this to a specific DNS server such as 10.0.0.10
    }
```

이전 예에서 같이 사용 하 여 ConfigMap을 만들기는 [kubectl 적용 configmap] [ kubectl-apply] 명령 및 YAML 매니페스트의 이름을 지정 합니다. 그런 다음 CoreDNS ConfigMap를 통해 다시 로드를 강제 합니다 [kubectl pod를 삭제] [ kubectl delete] 다시 만드는 데 Kubernetes 스케줄러에 대 한:

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="custom-proxy-server"></a>사용자 지정 프록시 서버

네트워크 트래픽에 대 한 프록시 서버를 지정 하는 경우 DNS에 맞게 ConfigMap을 만들 수 있습니다. 다음 예제에서는 업데이트 된 `proxy` 이름 및 고유한 환경에 대 한 값을 사용 하 여 주소입니다. 라는 파일을 만들고 `coredns-custom.json` 다음 예제에서는 구성을 붙여 넣습니다.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    .:53 {
        proxy foo.com 1.1.1.1
    }
```

이전 예제에서와 같이 사용 하 여 ConfigMap을 만들기는 [kubectl 적용 configmap] [ kubectl-apply] 명령 및 YAML 매니페스트의 이름을 지정 합니다. 그런 다음 CoreDNS ConfigMap를 통해 다시 로드를 강제 합니다 [kubectl pod를 삭제] [ kubectl delete] 다시 만드는 데 Kubernetes 스케줄러에 대 한:

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="use-custom-domains"></a>사용자 지정 도메인 사용

내부적으로 확인 될 수만 있는 사용자 지정 도메인을 구성 하려는 경우. 사용자 지정 도메인을 확인 하려는 하는 예를 들어 *puglife.local*, 유효한 최상위 도메인을 아닌 합니다. 사용자 지정 도메인 ConfigMap 없이 AKS 클러스터 주소를 확인할 수 없습니다.

다음 예제에서는 자체 환경에 대 한 값을 사용 하 여에 트래픽을 사용자 지정 도메인 및 IP 주소를 업데이트 합니다. 라는 파일을 만들고 `coredns-custom.json` 다음 예제에서는 구성을 붙여 넣습니다.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  puglife.server: |
    puglife.local:53 {
        errors
        cache 30
        proxy . 192.11.0.1  # this is my test/dev DNS server
    }
```

이전 예제에서와 같이 사용 하 여 ConfigMap을 만들기는 [kubectl 적용 configmap] [ kubectl-apply] 명령 및 YAML 매니페스트의 이름을 지정 합니다. 그런 다음 CoreDNS ConfigMap를 통해 다시 로드를 강제 합니다 [kubectl pod를 삭제] [ kubectl delete] 다시 만드는 데 Kubernetes 스케줄러에 대 한:

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="stub-domains"></a>스텁 도메인

CoreDNS 스텁 도메인 구성에 사용할 수 있습니다. 다음 예제에서는 자체 환경에 대 한 값을 사용 하 여 사용자 지정 도메인 및 IP 주소를 업데이트 합니다. 라는 파일을 만들고 `coredns-custom.json` 다음 예제에서는 구성을 붙여 넣습니다.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
    abc.com:53 {
        errors
        cache 30
        proxy . 1.2.3.4
    }
    my.cluster.local:53 {
        errors
        cache 30
        proxy . 2.3.4.5
    }
```

이전 예제에서와 같이 사용 하 여 ConfigMap을 만들기는 [kubectl 적용 configmap] [ kubectl-apply] 명령 및 YAML 매니페스트의 이름을 지정 합니다. 그런 다음 CoreDNS ConfigMap를 통해 다시 로드를 강제 합니다 [kubectl pod를 삭제] [ kubectl delete] 다시 만드는 데 Kubernetes 스케줄러에 대 한:

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="next-steps"></a>다음 단계

이 문서에서는 CoreDNS 사용자 지정에 대 한 몇 가지 예제 시나리오를 보여 주었습니다. CoreDNS 프로젝트에 대 한 자세한 내용은 [CoreDNS 업스트림 프로젝트 페이지][coredns]합니다.

핵심 네트워크 개념에 대 한 자세한 내용은 참조 하세요 [AKS에서 응용 프로그램에 대 한 개념을 네트워크][concepts-network]합니다.

<!-- LINKS - external -->
[kubednsblog]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[coredns]: https://coredns.io/
[corednsk8s]: https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns
[dnscache]: https://coredns.io/plugins/cache/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete

<!-- LINKS - external -->
[concepts-network]: concepts-network.md