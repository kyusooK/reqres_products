Started by GitHub push by kyusooK
Obtained Jenkinsfile from git https://github.com/kyusooK/reqres_products
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/Product Pipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential Github-Cred
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/Product Pipeline/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/kyusooK/reqres_products # timeout=10
Fetching upstream changes from https://github.com/kyusooK/reqres_products
 > git --version # timeout=10
 > git --version # 'git version 2.43.0'
using GIT_ASKPASS to set credentials Github-Cred
 > git fetch --tags --force --progress -- https://github.com/kyusooK/reqres_products +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision 3ca3dfa348111f9c1ed78d4761255ddc33370898 (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 3ca3dfa348111f9c1ed78d4761255ddc33370898 # timeout=10
Commit message: "Update Jenkinsfile"
 > git rev-list --no-walk ef5ff11c32e112296333b2d5e634864af61f0995 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Clone Repository)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential Github-Cred
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/Product Pipeline/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/kyusooK/reqres_products # timeout=10
Fetching upstream changes from https://github.com/kyusooK/reqres_products
 > git --version # timeout=10
 > git --version # 'git version 2.43.0'
using GIT_ASKPASS to set credentials Github-Cred
 > git fetch --tags --force --progress -- https://github.com/kyusooK/reqres_products +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision 3ca3dfa348111f9c1ed78d4761255ddc33370898 (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 3ca3dfa348111f9c1ed78d4761255ddc33370898 # timeout=10
Commit message: "Update Jenkinsfile"
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Maven Build)
[Pipeline] withMaven
[Pipeline] {
[Pipeline] sh
+ mvn package -DskipTests
Picked up JAVA_TOOL_OPTIONS: -Dmaven.ext.class.path="/var/lib/jenkins/workspace/Product Pipeline@tmp/withMavencf9ab15b/pipeline-maven-spy.jar" -Dorg.jenkinsci.plugins.pipeline.maven.reportsFolder="/var/lib/jenkins/workspace/Product Pipeline@tmp/withMavencf9ab15b" 
[INFO] [jenkins-event-spy] Generate /var/lib/jenkins/workspace/Product Pipeline@tmp/withMavencf9ab15b/maven-spy-20241115-032831-53511143124689044979292.log.tmp ...
[INFO] Scanning for projects...
[INFO] 
[INFO] --------------------< com.example:reqres-products >---------------------
[INFO] Building reqres-products 0.0.1.BUILD-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ reqres-products ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 6 resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ reqres-products ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- spring-cloud-contract-maven-plugin:2.1.3.RELEASE:generateTests (default-generateTests) @ reqres-products ---
[INFO] Skipping Spring Cloud Contract Verifier execution: skipTeststrue
[INFO] 
[INFO] --- spring-cloud-contract-maven-plugin:2.1.3.RELEASE:convert (default-convert) @ reqres-products ---
[INFO] Will use contracts provided in the folder [/var/lib/jenkins/workspace/Product Pipeline/src/test/resources/contracts]
[INFO] Copying Spring Cloud Contract Verifier contracts to [/var/lib/jenkins/workspace/Product Pipeline/target/stubs/META-INF/com.example/reqres-products/0.0.1.BUILD-SNAPSHOT/contracts]. Only files matching [.*] pattern will end up in the final JAR with stubs.
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Converting from Spring Cloud Contract Verifier contracts to WireMock stubs mappings
[INFO]      Spring Cloud Contract Verifier contracts directory: /var/lib/jenkins/workspace/Product Pipeline/src/test/resources/contracts
[INFO] Stub Server stubs mappings directory: /var/lib/jenkins/workspace/Product Pipeline/target/stubs/META-INF/com.example/reqres-products/0.0.1.BUILD-SNAPSHOT/mappings
[INFO] Creating new stub [/var/lib/jenkins/workspace/Product Pipeline/target/stubs/META-INF/com.example/reqres-products/0.0.1.BUILD-SNAPSHOT/mappings/rest/productGet.json]
[INFO] 
[INFO] --- maven-resources-plugin:3.1.0:testResources (default-testResources) @ reqres-products ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ reqres-products ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ reqres-products ---
[INFO] Tests are skipped.
[INFO] 
[INFO] --- spring-cloud-contract-maven-plugin:2.1.3.RELEASE:generateStubs (default-generateStubs) @ reqres-products ---
[INFO] Files matching this pattern will be excluded from stubs generation []
[INFO] Building jar: /var/lib/jenkins/workspace/Product Pipeline/target/reqres-products-0.0.1.BUILD-SNAPSHOT-stubs.jar
[INFO] 
[INFO] --- maven-jar-plugin:3.1.0:jar (default-jar) @ reqres-products ---
[INFO] Building jar: /var/lib/jenkins/workspace/Product Pipeline/target/reqres-products-0.0.1.BUILD-SNAPSHOT.jar
[INFO] 
[INFO] --- spring-boot-maven-plugin:2.1.1.RELEASE:repackage (repackage) @ reqres-products ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  7.872 s
[INFO] Finished at: 2024-11-15T03:28:39Z
[INFO] ------------------------------------------------------------------------
[INFO] [jenkins-event-spy] Generated /var/lib/jenkins/workspace/Product Pipeline@tmp/withMavencf9ab15b/maven-spy-20241115-032831-53511143124689044979292.log
[Pipeline] }
[withMaven] artifactsPublisher - Archive artifact pom.xml under com/example/reqres-products/0.0.1.BUILD-SNAPSHOT/reqres-products-0.0.1.BUILD-SNAPSHOT.pom
[withMaven] artifactsPublisher - Archive artifact target/reqres-products-0.0.1.BUILD-SNAPSHOT.jar under com/example/reqres-products/0.0.1.BUILD-SNAPSHOT/reqres-products-0.0.1.BUILD-SNAPSHOT.jar
[withMaven] artifactsPublisher - Archive artifact target/reqres-products-0.0.1.BUILD-SNAPSHOT-stubs.jar under com/example/reqres-products/0.0.1.BUILD-SNAPSHOT/reqres-products-0.0.1.BUILD-SNAPSHOT-stubs.jar
[withMaven] junitPublisher - Archive test results for Maven artifact com.example:reqres-products:jar:0.0.1.BUILD-SNAPSHOT generated by maven-surefire-plugin:test (default-test): target/surefire-reports/*.xml
[withMaven] junitPublisher - Jenkins JUnit Attachments Plugin not found, can't publish test attachments.
[withMaven] junitPublisher - Jenkins JUnit Flaky Test Handler Plugin not found, can't publish JUnit flaky tests reports.
Recording test results
No test report files were found. Configuration error?
None of the test reports contained any result
[Checks API] No suitable checks publisher found.
[withMaven] Jenkins FindBugs Plugin not found, don't display org.codehaus.mojo:findbugs-maven-plugin:findbugs results in pipeline screen.
[withMaven] jgivenPublisher - Jenkins JGiven Plugin not found, do not archive jgiven reports.
[withMaven] Jenkins Task Scanner Plugin not found, don't display results of source code scanning for 'TODO' and 'FIXME' in pipeline screen.
[withMaven] Publishers: Pipeline Graph Publisher: 2 ms, Generated Artifacts Publisher: 238 ms, Junit Publisher: 12 ms, Findbugs Publisher: 1 ms
[Pipeline] // withMaven
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Docker Build)
[Pipeline] script
[Pipeline] {
[Pipeline] isUnix
[Pipeline] withEnv
[Pipeline] {
[Pipeline] sh
+ docker build -t user19.azurecr.io/product:v8 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  54.18MB

Step 1/4 : FROM ghcr.io/gkedu/openjdk:8u212-jdk
 ---> 03b20c1fa768
Step 2/4 : COPY target/*SNAPSHOT.jar app.jar
 ---> e9d90171996f
Step 3/4 : EXPOSE 8080
 ---> Running in 5a5670cced1e
Removing intermediate container 5a5670cced1e
 ---> 9c8a4a31be4e
Step 4/4 : ENTRYPOINT ["java","-Xmx400M","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar","--spring.profiles.active=docker"]
 ---> Running in 0058c23128d3
Removing intermediate container 0058c23128d3
 ---> 886c08249e7d
Successfully built 886c08249e7d
Successfully tagged user19.azurecr.io/product:v8
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Azure Login)
[Pipeline] script
[Pipeline] {
[Pipeline] withCredentials
Masking supported pattern matches of $AZURE_CLIENT_SECRET
[Pipeline] {
[Pipeline] sh
+ az login --service-principal -u 5ae3b190-9406-40c1-9733-a922ad698da6 -p **** --tenant 29d166ad-94ec-45cb-9f65-561c038e1c7a
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "29d166ad-94ec-45cb-9f65-561c038e1c7a",
    "id": "919f662f-a6b5-46a5-8ca1-f192d3d24138",
    "isDefault": true,
    "managedByTenants": [],
    "name": "종량제4",
    "state": "Enabled",
    "tenantId": "29d166ad-94ec-45cb-9f65-561c038e1c7a",
    "user": {
      "name": "5ae3b190-9406-40c1-9733-a922ad698da6",
      "type": "servicePrincipal"
    }
  }
]
[Pipeline] }
[Pipeline] // withCredentials
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Push to ACR)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ az acr login --name user19
Login Succeeded
[Pipeline] sh
+ docker push user19.azurecr.io/product:v8
The push refers to repository [user19.azurecr.io/product]
2925fcb4a3f4: Preparing
a4e7d1c2f08d: Preparing
7955da51da82: Preparing
c64652873162: Preparing
a4e797bc3f15: Preparing
392f356944ff: Preparing
15210a41d4ee: Preparing
e2a8a00a83b2: Preparing
15210a41d4ee: Waiting
e2a8a00a83b2: Waiting
392f356944ff: Waiting
a4e7d1c2f08d: Layer already exists
c64652873162: Layer already exists
a4e797bc3f15: Layer already exists
7955da51da82: Layer already exists
15210a41d4ee: Layer already exists
392f356944ff: Layer already exists
e2a8a00a83b2: Layer already exists
2925fcb4a3f4: Pushed
v8: digest: sha256:18f25b1a0fd9aacc9ae3897e2860e3e2e6730f24ec155c75a0d733d8c332478d size: 2007
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (CleanUp Images)
[Pipeline] sh
+ docker rmi user19.azurecr.io/product:v8
Untagged: user19.azurecr.io/product:v8
Untagged: user19.azurecr.io/product@sha256:18f25b1a0fd9aacc9ae3897e2860e3e2e6730f24ec155c75a0d733d8c332478d
Deleted: sha256:886c08249e7da262461b92eadc41a5df4d1cee64b94c9e37fc4b0773f2c9e672
Deleted: sha256:9c8a4a31be4ee128f8264307b84a17d54b697f89b21a6b73ed5914e2dc0006f0
Deleted: sha256:e9d90171996f71e2ea778e713e312f4b67ba97aa038baa1b0d8f4458558589a9
Deleted: sha256:cb4c3514f4da5cf259296b2590b69c937528ac4f57081fc8fb9e97b575b5d395
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Update and Push to GitHub)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ sed -i s/latest/v8/g kubernetes/deploy.yaml
+ git config --global user.email rbtn110@uengine.org
+ git config --global user.name kyusooK
+ git add kubernetes/deploy.yaml
+ git commit -m Update deployment image tag to v8
[detached HEAD 5100e9c] Update deployment image tag to v8
 1 file changed, 1 insertion(+), 1 deletion(-)
+ git push origin HEAD:main
fatal: could not read Username for 'https://github.com': No such device or address
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
ERROR: script returned exit code 128
Finished: FAILURE
