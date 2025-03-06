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

## GitHub → Jenkins 연동

- GitHub와 Jenkins 연동은 Webhook으로 설정하였다.
- GitHub는 협업을 위해 **Organizations**로 생성하였다.

1. **GitHub Access Token 발급**
    1. Jenkins 에서 Github로 접근하기 위한 GitHub Access Token을 생성해줘야한다.
    2. personal access token (classic)으로 생성해주자
- Profile → settings → Developer Settings → Personal Access Tokens → Token classic
    - 아래와 같이 Repo 와 admin:repo_hook 권한을 체크 및 token 이름을 지정 후 생성

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/57639dd8-2487-49db-9276-bd070fdb3985/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/97bd61c9-463a-4bde-89e3-4f7f5814ff53/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/3c7661ba-edca-4f2f-8cb0-4f71ef45902a/Untitled.png)

1. **Token 값 복사**
- 아래와 같이 생성 후 Github Token 값을 확인할 수 있으며 생성 후 한번밖에 확인을 못하기에 복사하여 보관해야한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/9700055f-c22a-41ff-b52d-ee24f215daa7/Untitled.png)

1. **Jenkins Credentials 생성**
    1. Jenkins 에서 GitHub에서 발급 받은 Token 을 등록하여 사용하도록 설정
- Jenkins 관리 → Credentials → System( golobal) 클릭 → Add Credentials
- Kind : Username with Password
- Username : Github 이름
- Password : Githb Token
- ID : 임의로 지정(Pipeline 에서 해당 ID를 통해 Credentials을 사용한다)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/29ea791c-58ef-47d7-8be0-daf22b23ba95/Untitled.png)

1. Webhook 생성
    1. Github에서 commit이 되었을때 Jenkins에서 감지하도록 Webhook을 설정
- Webhook 연동할 Github Repo → Settings → Webhook → Add webhook
- Payload URL : Jenkins URL/github-webhook/
    - Jenkins URL 뒤 /github-webhook/ 는 필수로 붙여야 함

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/dbc22e3c-0032-4635-8825-4b03465ca9a2/Untitled.png)

1. **Pipeline 생성**
- Jenkins 새로운 item → Pipeline 생성

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/b6a44a2e-5f42-4442-a07e-9e0b1d4ffb27/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/07394389-2012-4d77-817f-e677ceb97fa4/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/ce8a7cc5-c806-47fe-9cd3-75d52fa5f92f/Untitled.png)

Pipeline

```bash
pipeline {
    agent any
stage('Checkout') {
    steps {
        echo "Git Checkout"
        git branch: 'main',
            credentialsId: 'jswon_github', // 생성한 credential의 ID
            url: 'https://github.com/cloit-jswan/jswan-project.git' // Build 하고자 하는 Repo
    }
}
}
```

※ PipeLine 생성 후 최초 1회는 **지금 빌드를 해줘야 Commit 이 일어났을때 자동으로 Pipeline이 돌아간다**

※ Build 를 했을때 jenkins의 **`/var/jenkins_home/workspace/project_name/` 디렉터리에 GihtHub에서 Build한 파일이 들어가게 된다.**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/beb3b52f-73e1-4a9d-92d4-7931825740db/d70b7aa0-9e46-45a1-b988-9ef2b9572d25/Untitled.png)
