ECS에서 **블루/그린 배포** 후 **이전 대상 그룹**이 **리스너**에서 자동으로 삭제되는 문제를 해결하려면, 배포가 끝난 후 **이전 대상 그룹을 자동으로 리스너에 추가**해야 한다. 이를 자동화하려면 **Lambda**를 사용하고, 자바로 코드를 작성하면 된다.

## 1. 목표

배포가 끝나면 **CloudWatch Events**가 배포 완료를 감지하고, **Lambda 함수**가 실행된다. 그럼 **이전 대상 그룹 ARN**을 **ELB 리스너**에 자동으로 추가한다.

## 2. 전체 흐름

1. **ECS 블루/그린 배포**가 진행된다.
2. 배포가 끝나면 **CodeDeploy**가 배포 상태를 변경하고, **CloudWatch Events**가 이를 감지한다.
3. **CloudWatch Events**는 배포 완료 이벤트를 감지하고, **Lambda 함수**가 실행된다.
4. **Lambda 함수**는 **ECS 서비스**의 **이전 대상 그룹 ARN**을 추적하고, 이를 **리스너에 추가**한다.

## 3. 단계별 작업

### Step 1: CloudWatch Events 규칙 만들기

배포 완료 이벤트가 발생하면 **CloudWatch Events**가 **Lambda 함수**를 트리거하도록 규칙을 만든다.

1. **CloudWatch Events 콘솔**에 들어간다.
2. **규칙 생성** 클릭 후, **이벤트 패턴**을 설정한다.
    - **Event Source**: `aws.codedeploy`
    - **Event Type**: `CodeDeploy Deployment State-change`
    - 배포 상태가 `SUCCEEDED`일 때만 트리거되도록 설정한다.
```json
{
  "source": [
    "aws.codedeploy"
  ],
  "detail-type": [
    "CodeDeploy Deployment State-change"
  ],
  "detail": {
    "state": [
      "SUCCEEDED"
    ]
  }
}
```
### Step 2: Lambda 함수 코드 작성 (Java)

Lambda 함수는 ECS 서비스의 **이전 대상 그룹**을 추적하고, 이를 **리스너에 추가**하는 작업을 한다.

#### Lambda 함수 예시 코드 (Java)
```java
package com.example;

import com.amazonaws.services.ecs.AmazonECS;
import com.amazonaws.services.ecs.AmazonECSClient;
import com.amazonaws.services.ecs.model.DescribeServicesRequest;
import com.amazonaws.services.ecs.model.DescribeServicesResponse;
import com.amazonaws.services.elbv2.AmazonElasticLoadBalancing;
import com.amazonaws.services.elbv2.AmazonElasticLoadBalancingClient;
import com.amazonaws.services.elbv2.model.ModifyListenerRequest;
import com.amazonaws.services.elbv2.model.ModifyListenerResponse;
import com.amazonaws.services.elbv2.model.Action;
import com.amazonaws.services.elbv2.model.ActionTypeEnum;
import com.amazonaws.services.elbv2.model.TargetGroup;

import java.util.List;

public class LambdaHandler {

    public void handleRequest(Map<String, Object> event, Context context) {
        // AWS 클라이언트 설정
        String region = "ap-northeast-2";
        String listenerArn = "<your-listener-arn>";  // 리스너 ARN
        String clusterName = "<your-cluster-name>";
        String serviceName = "<your-service-name>";
        
        AmazonECS ecsClient = AmazonECSClient.builder().region(region).build();
        AmazonElasticLoadBalancing elbv2Client = AmazonElasticLoadBalancingClient.builder().region(region).build();
        
        // 배포가 완료된 ECS 서비스 정보 가져오기
        String deploymentId = event.get("detail").get("deploymentId").toString();
        
        // ECS 서비스에서 이전 대상 그룹 정보 추출
        DescribeServicesRequest describeServicesRequest = new DescribeServicesRequest()
                .withCluster(clusterName)
                .withServices(serviceName);
        DescribeServicesResponse response = ecsClient.describeServices(describeServicesRequest);
        
        // 이전 대상 그룹 ARN 추출
        String previousTargetGroupArn = null;
        for (LoadBalancer loadBalancer : response.getServices().get(0).getLoadBalancers()) {
            previousTargetGroupArn = loadBalancer.getTargetGroupArn();
        }
        
        if (previousTargetGroupArn != null) {
            // 리스너에 이전 대상 그룹을 추가
            ModifyListenerRequest modifyListenerRequest = new ModifyListenerRequest()
                    .withListenerArn(listenerArn)
                    .withDefaultActions(new Action()
                            .withType(ActionTypeEnum.FORWARD)
                            .withTargetGroupArn(previousTargetGroupArn));
            
            ModifyListenerResponse modifyListenerResponse = elbv2Client.modifyListener(modifyListenerRequest);
            
            System.out.println("이전 대상 그룹 " + previousTargetGroupArn + "이 리스너에 추가되었습니다.");
        } else {
            System.out.println("이전 대상 그룹을 찾을 수 없습니다.");
        }
    }
}
```
### Step 3: Maven으로 JAR 파일 만들기

자바 코드를 작성한 후 **Maven**을 사용해서 **JAR 파일**로 패키징한다. 이 JAR 파일을 Lambda에 업로드한다.

#### Maven `pom.xml` 의존성
```maven
<dependencies>
    <dependency>
        <groupId>com.amazonaws</groupId>
        <artifactId>aws-java-sdk-ecs</artifactId>
        <version>1.12.1000</version>
    </dependency>
    <dependency>
        <groupId>com.amazonaws</groupId>
        <artifactId>aws-java-sdk-elasticloadbalancingv2</artifactId>
        <version>1.12.1000</version>
    </dependency>
</dependencies>
```
이후 **Maven** 명령어로 JAR 파일을 빌드한다.
```
mvn clean package
```
### Step 4: JAR 파일을 Lambda에 업로드

Lambda 콘솔에서 **JAR 파일**을 업로드한다.

1. **AWS Lambda 콘솔**에 들어간다.
2. **Java 8** 런타임을 선택한다.
3. **업로드된 파일**을 선택하고 **빌드한 JAR 파일**을 업로드한다.
4. **IAM 역할**을 설정하여 ECS와 ELB에 접근할 수 있도록 한다.

### Step 5: IAM 역할 설정

Lambda가 ECS와 ELB에 접근할 수 있도록 **IAM 역할**을 설정한다. 해당 역할에는 ECS와 ELB 리스너를 수정할 수 있는 권한이 필요하다.

IAM 역할 정책 예시
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecs:DescribeServices",
        "elbv2:ModifyListener"
      ],
      "Resource": "*"
    }
  ]
}
```
### Step 6: CloudWatch Events 설정

배포가 완료되면 Lambda 함수가 트리거되도록 **CloudWatch Events** 규칙을 설정한다.

1. **CloudWatch Events**에서 **규칙 생성**을 클릭한다.
2. 이벤트 패턴을 설정하고, 배포 상태가 **SUCCEEDED**일 때 Lambda가 호출되도록 한다.

## 4. 끝

이제 ECS 블루/그린 배포가 완료되면, **CloudWatch Events**와 **Lambda 함수**가 자동으로 **이전 대상 그룹을 추적**하고, **리스너에 추가**하는 작업을 수행한다. 이 과정은 **완전히 자동화**되어, 수동으로 처리할 필요가 없다.