네, **Kafka Schema Registry**는 별도의 서버로 구축해야 합니다. Schema Registry는 Kafka 클러스터와 독립적으로 동작하며, Kafka와 관련된 스키마를 중앙에서 관리하는 역할을 합니다. 이 서비스는 주로 **Confluent**에서 제공하는 Kafka 배포판에서 사용되며, Avro, Protobuf, JSON 스키마 등의 데이터를 관리할 수 있습니다.

### Schema Registry의 역할
- **스키마 관리**: 데이터가 Kafka 토픽으로 전송되기 전에 스키마를 중앙에서 관리하고, 이를 통해 스키마의 버저닝과 호환성을 유지합니다.
- **스키마 검증**: Kafka에 데이터를 전송하기 전에 Schema Registry를 통해 스키마의 유효성을 검사하고, 소비자가 데이터를 사용할 때 스키마와 일치하는지 확인합니다.
- **스키마 저장**: 스키마를 영구 저장소(예: Kafka 자체, HDFS, JDBC 호환 데이터베이스)에 저장하여 스키마의 일관성을 유지합니다.

### Schema Registry 서버 구축 방법
Schema Registry는 **독립적인 서비스**로, 다음과 같은 방식으로 구축할 수 있습니다:

1. **Standalone 방식**:
   - 별도의 서버나 컨테이너에 Schema Registry를 설치하여 운영할 수 있습니다.
   - 이 방법은 소규모 환경에서 쉽게 관리할 수 있는 방식입니다.

2. **Docker 방식**:
   - Docker 컨테이너를 사용하여 Schema Registry를 쉽게 배포할 수 있습니다.
   - Docker Compose 파일을 통해 Kafka, Zookeeper, Schema Registry 등을 함께 구성할 수 있습니다.

3. **Kubernetes 방식**:
   - Kubernetes 클러스터에서 Schema Registry를 배포하여, 확장성과 안정성을 높일 수 있습니다.
   - Helm 차트를 사용하면 Schema Registry를 쉽게 배포하고 관리할 수 있습니다.

### Schema Registry 설정 예시
Schema Registry를 설정할 때 필요한 기본 설정 파일은 `schema-registry.properties` 파일입니다. 예시는 다음과 같습니다:

```properties
# Kafka 브로커의 연결 정보
kafkastore.bootstrap.servers=PLAINTEXT://localhost:9092

# 스키마 정보를 저장할 Kafka 주제
kafkastore.topic=_schemas

# Schema Registry 서버의 호스트와 포트
listeners=http://0.0.0.0:8081

# 호환성 설정 (ex: backward, forward, full, none)
compatibility.level=backward
```

### Schema Registry 서버 시작
설정을 완료한 후, Schema Registry 서버를 시작할 수 있습니다:

```bash
./bin/schema-registry-start ./etc/schema-registry/schema-registry.properties
```

### Schema Registry의 운영 고려사항
- **고가용성**: 여러 Schema Registry 인스턴스를 클러스터링하여 고가용성을 확보할 수 있습니다. 이 경우, 각 인스턴스는 동일한 Kafka 클러스터와 연결됩니다.
- **보안**: Schema Registry 서버는 Kafka 클러스터와 마찬가지로 TLS 암호화와 SASL 인증을 설정하여 보안을 강화할 수 있습니다.
- **모니터링**: Schema Registry의 상태와 성능을 모니터링하기 위해 Prometheus와 Grafana 같은 도구를 사용할 수 있습니다.

### 결론
Kafka Schema Registry는 스키마 관리를 위해 별도로 구축되는 서버이며, 중앙 집중식 스키마 관리, 스키마 진화 및 호환성을 지원합니다. 운영 환경에 맞는 배포 방식을 선택하여 안정적으로 운영할 수 있습니다.
