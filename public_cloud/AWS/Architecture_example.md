![image](https://user-images.githubusercontent.com/62640332/155850621-cb887c4a-3f49-49dc-bf14-f18921d8f195.png)


![image](https://user-images.githubusercontent.com/62640332/155850660-2dd3f16b-fc15-4eb1-bc09-056d4705a5c4.png)

![image](https://user-images.githubusercontent.com/62640332/155850674-71d51630-043e-4e9c-9a0e-bc688884cb2b.png)

![image](https://user-images.githubusercontent.com/62640332/155851179-99fd617f-f433-44f7-8eb1-c6c5218df3a2.png)

![image](https://user-images.githubusercontent.com/62640332/155851208-23fae16c-2c75-48be-99e0-f831e478fb65.png)

![image](https://user-images.githubusercontent.com/62640332/155851266-602f854f-5263-44d5-88e1-48c9341eb96f.png)

제약사항 Bitbucket
CodePipeline 에서 코드를 받아와서 CodeBuild -> CodeDeploy 로 진행 할 수 있도록 할 수 있다. 하지만 CodePipeline 의 Source 공급설정을 보면 S3, Github, CodeCommit 은 가능한데, Bitbucket 에서 코드를 바로 가져올수가 없어, 위의 그림과 같이 CodePipeline 에서는 CodeDeploy 만을 활용하고, CodeBuild 는 개별로 Bitbucket 과 연결을 하였다.

Bitbucket pipeline
CodePipeline 에서 바로 Bitbucket 을 활용 할 수 없는 문제를 해결하기 위한 다른 방법으로 Bitbucket 에 pipeline 기능이 있다. 코드가 푸시되면 Bitbucket 이 특정 스크립트를 실행 할 수 있도록 기능을 지원한다. 해당 기능을 활용하여 코드를 S3로 업로드 하는 스크립트를 작성하여 S3에 올라온 코드를 캐치하여 CodePipeline 을 동작 하도록 할 수 있다. 하지만 본 글에서는 CodeBuild 에서 Bitbucket 의 코드를 가져와서 동작하도록 구축 하였다.

![image](https://user-images.githubusercontent.com/62640332/155851368-48332e11-90c4-4835-8a9d-bdf3838e137f.png)

![image](https://user-images.githubusercontent.com/62640332/155851387-843d7674-eb90-4dff-9578-7a37fb1b5d5c.png)

![image](https://user-images.githubusercontent.com/62640332/155851396-9fd75471-b3c6-40c6-b825-78c7c56e8c30.png)

- 1단계

: 일단, 제가 애용하는 툴은 vscode라서 vscode를 넣어놨습니다! 어떤 에디터에서든지 상관없이 코딩을 하고 깃허브에 push를 합니다.

- 2단계

: 깃헙 레파지토리에 푸쉬 이벤트가 일어나면 깃헙은 Travis CI에게 소스코드를 보냅니다. ( Travis CI에게 소스코드를 보내기 위해서는 Travis CI와 깃헙 레파지토리가 연동되어 있어야 합니다 )

- 3단계

: Travis CI는 깃헙 레파지토리가 보내준 소스코드들을 테스트하고 빌드해줍니다.

- 4단계

: 4단계에서는 빌드된 파일들을 Travis CI가 AWS S3에 업로드를 합니다.

Travis CI는 이름대로 CI라서 코드 배포를 못합니다! 그렇기 때문에 코드를 배포할 수 있겠금 무언가가 도와줘야합니다. 그 무언가가 AWS 입니다! AWS에는 Code Deploy라는 훌륭한 기능들과 S3라는 저장소가 있습니다.

- 5단계

: Travis CI가 S3에 빌드된 파일을 보내준후 이제 배포해~ 라는 명령어 스크립트를 CodeDeploy에게 전달합니다.

- 6단계

: CodeDeploy는 Travis CI로 부터 온 배포해 라는 명령어 스크립트를 실행해 S3로 부터 파일을 가져온 후 배포 작업을 시작합니다.

- 7단계

: CodeDeploy는 AWS거라서 EC2랑도 연동이 됩니다. CodeDeploy는 S3에서 받은 파일을 해당 인스턴스에 넣어준후 EC2 내부에 정의된 배포 스크립트를 실행해 줍니다.

- 8단계

: CodeDeploy가 EC2 내부에 정의된 배포스크립트를 실행해 줌으로 EC2 내부적으로 배포를 진행합니다. EC2 내부에 정의된 배포스크립트는 무중단배포를 가능하게끔 스크립트가 작성되어 있습니다.


![image](https://user-images.githubusercontent.com/62640332/155851448-083c5585-4275-4698-bef5-5335af003529.png)

![image](https://user-images.githubusercontent.com/62640332/155851472-8a43203f-fe56-4b4a-806a-21924083caaa.png)


![image](https://user-images.githubusercontent.com/62640332/155851554-0a76827e-83e2-4d32-9d5e-08225bcb7c82.png)