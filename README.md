# 📦 Inventory Management System (Spring Boot + Hibernate + MySQL)

A high-performance REST API for managing inventory operations (CRUD, stock adjustments, search, analytics). Engineered with indexing, query optimization, and concurrency patterns to handle heavy transactional load.

---

## 🌟 Value Proposition

| Focus | Implementation |
| ----- | -------------- |
| Data Integrity | ACID transactions via Spring + JPA |
| Performance | Indexed queries, DTO projections, pagination |
| Concurrency | Optimistic locking (version fields) + strategic multithreading for batch imports |
| Scalability | Stateless REST + Docker + AWS EC2 deployment |
| Clean Architecture | Layered design with separation of concerns |
| Monitoring | Structured logs + potential metrics hooks |

---

## 🧱 Architecture

```
Client
  |
  v
+----------+       +-----------------+
| REST API | ----> | Service Layer    |
+----+-----+       +---------+-------+
     |                         |
     v                         v
+-----------+          +---------------+
| Repository| <------> |  MySQL (RDS)  |
+-----------+          +---------------+

 Batch Processor / Import Workers (Executor) -> writes via transactional service
```

---

## 🗂️ Suggested Structure

```
inventory-management-system
 ├─ src/main/java/com/example/inventory
 │   ├─ config/
 │   ├─ domain/          # Entities: Product, Category, StockMovement
 │   ├─ dto/
 │   ├─ repository/
 │   ├─ service/         # ProductService, StockService, ImportService
 │   ├─ web/             # Controllers
 │   ├─ batch/           # Multithreaded import & sync tasks
 │   ├─ exception/
 │   └─ util/
 ├─ src/main/resources/
 │   ├─ application.yml
 │   └─ schema.sql / data.sql (optional)
 ├─ Dockerfile
 ├─ pom.xml
 └─ README.md
```

---

## 🗃️ Data Model (Sample)

`Product`
- id (PK)
- sku (unique, indexed)
- name (indexed)
- category_id (FK)
- quantity
- price
- version (optimistic locking)
- created_at / updated_at

`StockMovement`
- id
- product_id
- delta (+/-)
- reason (SALE, RETURN, ADJUSTMENT)
- created_at

Indexes:
```sql
CREATE INDEX idx_product_sku ON product (sku);
CREATE INDEX idx_product_name ON product (name);
CREATE INDEX idx_stockmovement_product ON stock_movement (product_id);
```

---

## ⚙️ Performance Techniques

| Technique | Description |
| --------- | ----------- |
| Optimistic locking | Prevent overwrites in concurrent updates |
| Bulk operations batching | Chunk size configurable (e.g., 100 rows) |
| Read projections | Using interface-based DTO queries for list endpoints |
| Proper indexing | Fast lookup by SKU, name |
| Connection pool tuning | HikariCP configuration |
| Pagination | `/products?page=0&size=50&sort=name,asc` |

---

## 🔐 Security (Optional Add-On)

If combined with auth service:
- Validate JWT via a gateway / filter
- Enforce roles for stock adjustment endpoints

---

## 🚀 Running Locally

Prerequisites: Java 17+, Maven, MySQL.

`application.yml` snippet:
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/inventorydb
    username: root
    password: yourpassword
  jpa:
    hibernate:
      ddl-auto: update
    properties:
      hibernate.order_updates: true
      hibernate.jdbc.batch_size: 50
      hibernate.generate_statistics: false
server:
  port: 8082
logging:
  level:
    org.hibernate.SQL: warn
```

Run:
```
mvn clean package -DskipTests
mvn spring-boot:run
```

Docker (concept):
```
docker build -t inventory-service:latest .
docker run -p 8082:8082 --env-file .env inventory-service:latest
```

---

## 🧪 API Endpoints (Sample)

| Method | Path | Description |
| ------ | ---- | ----------- |
| POST | /api/products | Create product |
| GET  | /api/products | List (pagination + filters) |
| GET  | /api/products/{id} | Get details |
| PATCH| /api/products/{id}/stock | Adjust stock |
| GET  | /api/products/search?sku=ABC123 | Lookup product |
| GET  | /api/analytics/low-stock | Low stock report |

Create product:
```bash
curl -X POST http://localhost:8082/api/products \
  -H "Content-Type: application/json" \
  -d '{"sku":"SKU-1001","name":"Gaming Mouse","categoryId":1,"quantity":50,"price":1499.00}'
```

Adjust stock:
```bash
curl -X PATCH http://localhost:8082/api/products/5/stock \
  -H "Content-Type: application/json" \
  -d '{"delta": -2, "reason": "SALE"}'
```

---

## 🔄 Concurrency & Batch Imports

Example approach:
- `ImportService` parses CSV and submits `Callable` tasks to a fixed thread pool.
- Each task runs inside a Spring-managed transaction.
- Conflict detection via `@Version` field on `Product`.

Pseudocode:
```java
executor.invokeAll(
  chunks.stream()
    .map(chunk -> (Callable<Void>) () -> { productImporter.importChunk(chunk); return null; })
    .toList()
);
```

---

## 🧪 Testing Strategy

| Layer | Approach |
| ----- | -------- |
| Repositories | @DataJpaTest |
| Services | Mocked repos + concurrency tests |
| API | MockMvc integration |
| Performance | JMeter / Gatling idea (baseline throughput) |

---

## 📈 Potential Enhancements

| Feature | Benefit |
| ------- | ------- |
| Redis caching for lookups | Faster product catalog queries |
| Event sourcing (Kafka) | Audit & replay of stock changes |
| OpenAPI docs | Consumer clarity |
| Metrics (Micrometer) | Throughput & error insight |
| Bulk CSV export/ import endpoints | Operational efficiency |
| Multi-tenant schema strategy | SaaS readiness |

---

## 🧾 License

MIT (Add LICENSE file if desired.)

---

## 👤 Author

Developed by **Rasid Raja Khan** – Focused on robust, scalable backend engineering with attention to performance & data integrity.

Let’s connect if you’re building high-throughput systems!

---
