# Logging open source
+ Loki(3.4.3)
+ Protail(3.4.3)
+ Grafana(9.0.5)
+ Prometheus(2.53.4)
<br>


# Loki
+ 로그 집계 시스템으로, 로깅 및 이벤트 데이터를 수집, 저장 및 검색하기 위한 오픈 소스 플랫폼
  
### Download
<pre><code>https://github.com/grafana/loki/releases</code></pre>
(3.4.3기준)Assets에서 loki-windows-amd64.exe.zip 파일 원하는 폴더(C:/project/tool/loki)에 다운로드 및 압축 해제

### loki-config.yaml 생성 및 작성
+ 원하는 폴더(C:/project/tool/loki)에 loki-config.yaml 생성하여 아래내용 적용 
<pre><code>auth_enabled: false

server:
  # 서버 listen 포트 설정
  http_listen_port: 3100
  # 관리포트 비활성화
  grpc_listen_port: 0

ingester:
  lifecycler:
    ring:
      kvstore:
        store: inmemory
      replication_factor: 1

schema_config:
  configs:
    - from: 2020-10-24
      # 인덱스 저장소 유형
      store: boltdb-shipper
      # 객체 저장소 유형
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        # 인덱스 파일 생성 주소
        period: 24h

storage_config:
  boltdb_shipper:
    active_index_directory: C:/project/tool/loki/storage/index
    cache_location: C:/project/tool/loki/storage/index_cache
  filesystem:
    directory: C:/project/tool/loki/storage/chunks

limits_config:
  allow_structured_metadata: false

compactor:
  working_directory: C:/project/tool/loki/work</code></pre>
  
### 실행
+ C:/project/tool/loki 로 이동하여 cmd 창 열어서 아래 명령어 실행
<pre><code>loki-windows-amd64.exe --config.file=loki-config.yaml -config.expand-env=true</code></pre>
<br>


# Protail
+ Loki를 위해 만들어진 로그 수집 도구

### Download
<pre><code>https://github.com/grafana/loki/releases</code></pre>
(3.4.3기준)Assets에서 promtail-windows-amd64.exe.zip 파일 원하는 폴더(C:/project/tool/promtail)에 다운로드 및 압축 해제

### promtail-config.yaml 생성 및 작성
+ 원하는 폴더(C:/project/tool/promtail)에 promtail-config.yaml 생성하여 아래내용 적용
<pre><code>server:
  http_listen_port: 9080

positions:
  filename: C:/project/tool/loki/conf/positions.yaml

clients:
  # Loki 서버 주소
  - url: http://127.0.0.1:3100/loki/api/v1/push

scrape_configs:
  - job_name: gateway
    static_configs:
      - targets:
          - localhost
        labels:
          #label name
          job: gateway-app
          #로그 파일 경로
          __path__: C:/project/logs/info/*.log</code></pre>

### 실행
+ C:/project/tool/promtail 로 이동하여 cmd 창 열어서 아래 명령어 실행
<pre><code>promtail-windows-amd64.exe --config.file=promtail-config.yaml</code></pre>
<br>


# Grafana
+ 오픈 소스 시각화 도구로, 다양한 데이터 소스에서 데이터를 수집하고 이를 차트, 그래프, 알림 대시보드로 시각화해 주는 도구
  
### Download
<pre><code>https://grafana.com/grafana/download?platform=windows</code></pre>
zip 파일 원하는 폴더(C:/project/tool/grafana)에 다운로드 및 압축 해제

### 실행
+ C:/project/tool/grafana/bin 로 이동하여 grafana-server.exe 실행
+ http://localhost:3000/login 접속 admin/admin으로 로그인

