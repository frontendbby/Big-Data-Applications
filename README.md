# Big-Data-Applications

# Pipeline Robusto de Auditoría y Trazabilidad para Big Data

## 📋 Descripción Técnica

Sistema enterprise-grade de auditoría y validación de integridad para pipelines de datos, implementado en Python 3.8+ con arquitectura modular y cumplimiento de estándares ISO 8601 para trazabilidad temporal.

## 🏗️ Arquitectura

### Core Components

**Integrity Validation Engine**
- Hashing criptográfico dual: SHA-256 (primary) + MD5 (redundancy)
- Verificación de integridad con método `verify_integrity()` para detección de alteraciones
- Timestamps ISO 8601 en todas las operaciones para trazabilidad forense

**Structured Metadata Framework**
- `HashMetadata`: Contenedor inmutable de hashes con timestamp de generación
- `DataQualityMetrics`: Métricas automáticas de calidad (null ratio, duplicate detection, completeness score, dtype profiling)
- `SourceMetadata`: Registro completo de fuente incluyendo encoding, size, hash chain, y metadata adicional extensible
- `AuditRecord`: Registro maestro con execution graph, environment fingerprint, y status tracking

**Multi-Format Data Source Processor**
- **TXT**: Line counting, character encoding detection, content normalization
- **CSV**: Pandas-based ingestion con quality profiling (null analysis, duplicate detection, schema inference)
- **HTML**: BeautifulSoup parsing con tag counting, text extraction, y structure analysis
- Arquitectura extensible para JSON, XML, Parquet mediante factory pattern

**Logging & Observability**
- Dual output: rotating file handler (`audit_pipeline.log`) + stdout stream
- Structured logging con niveles jerárquicos (INFO/WARNING/ERROR)
- Contextual metadata en cada log entry para debugging distribuido

## 🔬 Características Técnicas

### Garantías de Integridad
```python
# Doble hash para cross-validation
SHA-256: Cryptographically secure (256-bit)
MD5: Fast checksum (128-bit) para backward compatibility
```

### Métricas de Calidad de Datos (CSV)
- **Completeness Ratio**: `(1 - null_count / total_cells)`
- **Duplicate Detection**: Row-level hash comparison
- **Type Inference**: Automatic dtype profiling per column
- **Threshold Alerting**: Warnings cuando completeness < 0.95

### Trazabilidad Forense
- **Audit ID Generation**: `AUDIT_{timestamp}_{sha256_suffix}` para unicidad global
- **Environment Fingerprinting**: Python version, OS, architecture, user context
- **Execution Metadata**: Pipeline version semántica, timestamp ISO 8601, source URLs
- **State Persistence**: JSON serialization con indent=2 para human readability

### Gestión de Estados
```python
AuditStatus.SUCCESS  # 100% success rate
AuditStatus.WARNING  # Partial success (0 < failures < total)
AuditStatus.FAILURE  # Complete failure (0 successful sources)
```

## 📊 Output Schema

### JSON Audit Record
```json
{
  "audit_id": "AUDIT_20260117_143052_a3f2d1e8",
  "pipeline_version": "1.0.0",
  "execution_timestamp": "2026-01-17T14:30:52.123456",
  "sources_processed": [
    {
      "source_id": "CSV_001",
      "source_name": "registros_eventos.csv",
      "source_type": "csv",
      "source_url": "https://...",
      "size_bytes": 45231,
      "encoding": "utf-8",
      "processing_timestamp": "2026-01-17T14:30:53.789012",
      "hash_metadata": {
        "sha256": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
        "md5": "d41d8cd98f00b204e9800998ecf8427e",
        "timestamp": "2026-01-17T14:30:53.789012",
        "algorithm_version": "SHA-256, MD5"
      },
      "quality_metrics": {
        "total_records": 1000,
        "null_count": 25,
        "duplicate_count": 5,
        "data_types": {
          "timestamp": "datetime64[ns]",
          "event_id": "int64",
          "description": "object"
        },
        "completeness_ratio": 0.975
      },
      "additional_info": {
        "rows": 1000,
        "columns": 3,
        "column_names": ["timestamp", "event_id", "description"]
      }
    }
  ],
  "audit_status": "success",
  "total_sources": 3,
  "successful_sources": 3,
  "failed_sources": 0,
  "warnings": ["CSV_001: Completeness ratio below threshold (97.5% < 95%)"],
  "errors": [],
  "environment_info": {
    "python_version": "3.11.4",
    "platform": "Linux-5.15.0-x86_64",
    "machine": "x86_64",
    "processor": "Intel(R) Core(TM) i7",
    "execution_user": "data_engineer"
  }
}
```

## 🛠️ Stack Tecnológico

**Core Dependencies**
- `pandas >= 1.5.0`: DataFrame processing y quality profiling
- `requests >= 2.28.0`: HTTP client con timeout handling
- `beautifulsoup4 >= 4.11.0` + `lxml`: HTML parsing engine
- `hashlib` (stdlib): Cryptographic hash functions
- `dataclasses` (stdlib): Immutable data containers
- `logging` (stdlib): Structured logging framework

**Design Patterns**
- Factory Pattern: Multi-format processor selection
- Strategy Pattern: Format-specific processing algorithms
- Builder Pattern: Audit record construction
- Singleton Pattern: Logging configuration

## 🔒 Compliance & Standards

- **ISO 8601**: Timestamp formatting para interoperabilidad
- **SHA-256**: NIST FIPS 180-4 compliant hashing
- **Data Lineage**: Full backward traceability desde output a source
- **Immutable Audit Trail**: Write-once JSON logs para forensic analysis
- **Environment Reproducibility**: Complete execution context capture

## 🚀 Performance Characteristics

- **Memory Efficient**: Streaming processing para archivos grandes
- **Concurrent Safe**: Thread-safe logging con queue handlers
- **Idempotent**: Múltiples ejecuciones con mismo input → mismo hash
- **Fault Tolerant**: Try-catch wrapping con graceful degradation
- **Scalable**: O(n) complexity para n sources

## 📈 Use Cases

1. **Investigación Académica**: Reproducibilidad científica con audit trails
2. **Data Governance**: Compliance con normativas de trazabilidad
3. **MLOps**: Validación de datasets pre-entrenamiento
4. **Data Warehousing**: ETL integrity verification
5. **Forensic Analysis**: Post-mortem debugging de data pipelines

## 🎓 Contexto Académico

**Institución**: Universidad Politécnica Metropolitana de Hidalgo  
**Programa**: Maestría en Inteligencia Artificial - Big Data  
**Autores**: Sánchez Ríos José Luis (253220161), Santos Martínez Víctor Manuel (253220020)  
**Instructor**: Dr. Jaime Aguilar Ortiz  
**Fecha**: 17 de enero de 2026

Implementa best practices de Data Engineering conforme a estándares académicos y profesionales para desarrollo de habilidades en pipeline design, data quality management, y audit trail implementation.

## 📝 Licencia

Código educativo para uso académico en el contexto de la asignatura Big Data - Cuatrimestre 2.
