# Docker
+ gradle build
<pre><code>./gradlew clean build</code></pre>
+ docker build
<pre><code>docker build -f Dockerfile -t api-gateway:0.0.1 .</code></pre>
+ container 실행
<pre><code>docker run -d --name api-gateway -p 8080:8080 api-gateway:0.0.1 -e USER_PROFILE=local</code></pre>
- - -
# Kubernetes
