## Team Leader 권한 정책 
### **1. AmazonEC2ContainerRegistryFullAccess**
- **무엇을 할 수 있나?**  
    ECR(Elastic Container Registry)에서 컨테이너 이미지를 **업로드, 다운로드, 삭제**할 수 있다.
- **왜 필요한가?**  
    컨테이너 이미지를 관리하고, 이를 다양한 환경에서 사용할 수 있게 해준다.

---
### **2. AmazonEC2ContainerRegistryReadOnly**
- **무엇을 할 수 있나?**  
    ECR에서 이미지를 **조회**하고 **다운로드**할 수 있다. 이미지를 수정하거나 삭제할 수는 없다.
- **왜 필요한가?**  
    이미지를 다운로드하여 사용하는 역할을 맡은 사람에게 필요한 권한.

---
### **3. AmazonEKSClusterPolicy**
- **무엇을 할 수 있나?**  
    EKS(Elastic Kubernetes Service) 클러스터를 **생성, 관리**하고, 클러스터의 상태를 확인할 수 있다.
- **왜 필요한가?**  
    Kubernetes 클러스터를 관리하고 설정하는 데 필요한 권한.
---
### **4. AmazonEKSServicePolicy**
- **무엇을 할 수 있나?**  
    EKS 서비스에 대해 **작업을 추가**하거나 **서비스와 연관된 작업**을 수행할 수 있다.
- **왜 필요한가?**  
    클러스터 내에서 서비스나 워커 노드를 추가하고 관리하는 역할을 담당.
---
### **5. AmazonEKSWorkerNodePolicy**
- **무엇을 할 수 있나?**  
    EKS 클러스터에서 **실행 중인 워커 노드**에 대한 권한을 부여.
- **왜 필요한가?**  
    애플리케이션을 실행하는 워커 노드가 클러스터와 상호작용할 수 있도록 한다.
---
### **6. AWSCodeBuildAdminAccess**
- **무엇을 할 수 있나?**  
    AWS CodeBuild를 **관리**하고, 빌드 프로세스를 설정 및 수정할 수 있다.
- **왜 필요한가?**  
    애플리케이션을 빌드하고 Docker 이미지를 생성하는 과정에서 필요한 권한.
---
### **7. AWSCodePipeline_FullAccess**
- **무엇을 할 수 있나?**  
    AWS CodePipeline을 **설정하고 관리**할 수 있다. 소스 코드부터 배포까지 파이프라인을 구축할 수 있다.
- **왜 필요한가?**  
    CI/CD 파이프라인을 설정하고 운영하는 역할을 맡은 사람에게 필요한 권한.
---
### **8. AWSCodePipeline_ReadOnlyAccess**
- **무엇을 할 수 있나?**  
    CodePipeline의 상태를 **조회**할 수 있다. 수정이나 실행은 불가능.
- **왜 필요한가?**  
    파이프라인의 진행 상태나 설정을 확인만 해야 하는 경우 유용.
---
### **9. ElasticLoadBalancingFullAccess**
- **무엇을 할 수 있나?**  
    ALB(Application Load Balancer)나 NLB(Network Load Balancer)를 **설정하고 관리**할 수 있다.
- **왜 필요한가?**  
    애플리케이션의 트래픽을 적절히 분산시키기 위해 로드 밸런서를 설정하고 관리하는 데 필요한 권한.
---