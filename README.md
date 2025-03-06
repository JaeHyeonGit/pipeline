# pipeline
![image](https://github.com/user-attachments/assets/2673341c-8c8c-4e3c-8804-b3084c81e92f)

1. 개발자가 Source를 변경하여 GitHub로 Push
2. 변경된 Source 코드와 Dockerfile 은 Jenkins로 Build
3. 변경된 Source 코드가 포함된 Python 이미지를 Dockerfile로 이미지Build
4. 생성된 이미지를 Amazon ECR로 Push
5. Kustomize를 사용하여 GitHub에 있는 Manifest File의 이미지 태그 버전을 ECR로 빌드한 버전으로 수정
6. ArgoCD에서는 변경된 Manifest File을 기반으로 쿠버네티스 자원을 생성


Pipeline 단계 별 설명

# CI 구성_CheckOut

![image](https://github.com/user-attachments/assets/a05f9df3-ee66-4d80-ae0f-77ee59268ff4)
