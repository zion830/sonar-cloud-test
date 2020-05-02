## Automatic Analysis
- default branch 변경, pull request 발생 시 자동 분석
- analysis configuration : `.sonarcloud.properties`에서 추가 옵션 설정 가능
```
# Path to sources
#sonar.sources=.
#sonar.exclusions=
#sonar.inclusions=

# Path to tests
#sonar.tests=
#sonar.test.exclusions=
#sonar.test.inclusions=

# Source encoding
#sonar.sourceEncoding=UTF-8

# Exclusions for copy-paste detection
#sonar.cpd.exclusions=
```

## CI-based Analysis (Maven project)
- Automatic Analysis, CI-based Analysis 동시 사용은 불가능
    - 사용하려면 둘 중 하나는 비활성화 해야됨
- 실행할 명령어
    - property로 `projectKey`, `organizationKey`, `host`, `sonarcloud authentication key` 설정해야됨
    - `SONAR_TOKEN`은 repository Setting > Secrets 페이지에서 환경 변수로 설정
    - `GITHUB_TOKEN`은 자동으로 생성됨
```
./mvnw -B verify sonar:sonar \
   -Dsonar.projectKey=<project key> \
   -Dsonar.organization=<organization key> \
   -Dsonar.host.url=https://sonarcloud.io \
   -Dsonar.login=<sonarcloud key>
```

- job script 예시
```
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: Set Up JDK
      uses: actions/setup-java@v1
      with:
        java-version: '11'
    - name: Run SonarCloud
      run: ./mvnw -B verify sonar:sonar -Dsonar.projectKey=zion830_sonar-cloud-test -Dsonar.organization=zion830-sonar-cloud-key -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=$SONAR_TOKEN
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```
