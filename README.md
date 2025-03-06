![image](https://github.com/user-attachments/assets/5ab44506-479e-41e4-8d12-a09932c7f409)# pipeline
![image](https://github.com/user-attachments/assets/2673341c-8c8c-4e3c-8804-b3084c81e92f)

1. 개발자가 Source를 변경하여 GitHub로 Push
2. 변경된 Source 코드와 Dockerfile 은 Jenkins로 Build
3. 변경된 Source 코드가 포함된 Python 이미지를 Dockerfile로 이미지Build
4. 생성된 이미지를 Amazon ECR로 Push
5. Kustomize를 사용하여 GitHub에 있는 Manifest File의 이미지 태그 버전을 ECR로 빌드한 버전으로 수정
6. ArgoCD에서는 변경된 Manifest File을 기반으로 쿠버네티스 자원을 생성


Pipeline 단계 별 설명

# CI 구성_CheckOut
https://www.notion.so/CI-_CheckOut-e9b42cd2ffc34990834a6a134ec4fae7?pvs=4

# **CI 구성_Build**
https://www.notion.so/CI-_Build-befc136273ec47a484a142394709ee97?pvs=4

# CI 구성_Push
https://www.notion.so/CI-_Push-18ba28f796f149e58bebc074476168a1?pvs=4

# CI 구성_Modify Manifests & Git Push
https://www.notion.so/CI-_Modify-Manifests-Git-Push-34d36a13eb604098af047d8f3290004e?pvs=4

# CD 구성_ArgoCD
https://www.notion.so/CD-_ArgoCD-8243c6f52262436885a1a598ebf59113?pvs=4

# ArgoCD_TroubleShooting
https://www.notion.so/ArgoCD_TroubleShooting-505d72aa45004960a20a53377ce456f3?pvs=4
