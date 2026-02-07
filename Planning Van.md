# Planning Van - Fullstack Lead

## Dokumen Perencanaan Detail untuk Van

**Role:** Fullstack Lead (Backend Focus, Flutter, ML)  
**Periode:** 18 Februari - 10 Juni 2026 (16 Minggu)

---

## Ringkasan Tanggung Jawab

| Area            | Deskripsi                                                 |
| --------------- | --------------------------------------------------------- |
| **Backend API** | Arsitektur, development, dan maintenance seluruh REST API |
| **Database**    | Schema design, migrations, optimization                   |
| **Mobile App**  | Flutter development untuk aplikasi kasir                  |
| **ML Pipeline** | Training dan deployment model prediksi & fraud detection  |
| **DevOps**      | CI/CD, deployment, monitoring                             |
| **Tech Lead**   | Code review, technical decisions, mentoring               |

---

## Tech Stack yang Digunakan

| Layer             | Teknologi             | Versi  |
| ----------------- | --------------------- | ------ |
| Backend Framework | FastAPI               | 0.109+ |
| Python            | Python                | 3.11+  |
| Database          | PostgreSQL            | 15+    |
| ORM               | SQLAlchemy            | 2.0+   |
| Migration         | Alembic               | 1.13+  |
| Mobile            | Flutter               | 3.22+  |
| State Management  | Riverpod / Provider   | Latest |
| Local DB          | Drift (SQLite)        | Latest |
| ML                | Prophet, Scikit-learn | Latest |
| ML Tracking       | MLflow                | Latest |

---

## Fase 0: Setup (Minggu 1)

**Periode:** 18-25 Februari 2026

### Checklist Tasks

#### Day 1-2: Repository & Project Structure

- [ ] Buat repository Git (GitHub/GitLab)
- [ ] Setup monorepo structure:
    ```
    aipos/
    ├── apps/
    │   ├── backend/
    │   └── mobile/
    ├── ml/
    ├── database/
    ├── docs/
    └── infrastructure/
    ```
- [ ] Setup `.gitignore` untuk Python, Flutter, Node
- [ ] Buat `README.md` dengan setup instructions
- [ ] Setup branch protection rules (main, develop)

#### Day 2-3: Backend Project Setup

- [ ] Initialize FastAPI project dengan Poetry:
    ```bash
    cd apps/backend
    poetry init
    poetry add fastapi uvicorn sqlalchemy alembic psycopg2-binary
    poetry add pydantic pydantic-settings python-jose passlib bcrypt
    poetry add python-multipart aiofiles httpx
    poetry add --group dev pytest pytest-asyncio pytest-cov ruff mypy
    ```
- [ ] Setup project structure:
    ```
    apps/backend/
    ├── app/
    │   ├── __init__.py
    │   ├── main.py
    │   ├── config.py
    │   ├── dependencies.py
    │   ├── routers/
    │   │   ├── __init__.py
    │   │   ├── auth.py
    │   │   ├── users.py
    │   │   ├── products.py
    │   │   ├── categories.py
    │   │   ├── transactions.py
    │   │   ├── shifts.py
    │   │   ├── inventory.py
    │   │   ├── reports.py
    │   │   └── payments.py
    │   ├── models/
    │   │   ├── __init__.py
    │   │   ├── user.py
    │   │   ├── product.py
    │   │   ├── transaction.py
    │   │   └── schemas.py
    │   ├── services/
    │   │   ├── __init__.py
    │   │   ├── auth_service.py
    │   │   ├── transaction_service.py
    │   │   ├── inventory_service.py
    │   │   ├── fraud_service.py
    │   │   └── blockchain_service.py
    │   ├── db/
    │   │   ├── __init__.py
    │   │   ├── database.py
    │   │   └── repositories/
    │   └── middleware/
    │       ├── __init__.py
    │       └── auth.py
    ├── tests/
    ├── alembic/
    ├── pyproject.toml
    └── Dockerfile
    ```
- [ ] Setup environment variables (`.env.example`)
- [ ] Setup logging configuration

#### Day 3-4: Database Setup

- [ ] Setup Docker Compose untuk PostgreSQL:
    ```yaml
    # docker-compose.yml
    services:
        db:
            image: postgres:15
            environment:
                POSTGRES_USER: aipos
                POSTGRES_PASSWORD: aipos_dev
                POSTGRES_DB: aipos_db
            ports:
                - "5432:5432"
            volumes:
                - postgres_data:/var/lib/postgresql/data
    ```
- [ ] Buat database schema lengkap (lihat PRD)
- [ ] Setup Alembic migrations
- [ ] Buat initial migration
- [ ] Buat seed data untuk testing

#### Day 4-5: Flutter Project Setup

- [ ] Initialize Flutter project:
    ```bash
    cd apps/mobile
    flutter create --org com.aipos aipos_mobile
    ```
- [ ] Setup dependencies (`pubspec.yaml`):
    ```yaml
    dependencies:
        flutter_riverpod: ^2.4.0
        dio: ^5.4.0
        drift: ^2.14.0
        sqlite3_flutter_libs: ^0.5.18
        path_provider: ^2.1.1
        mobile_scanner: ^3.5.5
        shared_preferences: ^2.2.2
        connectivity_plus: ^5.0.2
        intl: ^0.18.1
        printing: ^5.11.1
        go_router: ^13.0.0
    ```
- [ ] Setup project structure:
    ```
    apps/mobile/lib/
    ├── main.dart
    ├── app.dart
    ├── config/
    │   ├── routes.dart
    │   └── theme.dart
    ├── core/
    │   ├── constants/
    │   ├── utils/
    │   └── extensions/
    ├── data/
    │   ├── local/
    │   │   ├── database.dart
    │   │   └── tables/
    │   ├── remote/
    │   │   ├── api_client.dart
    │   │   └── endpoints/
    │   └── repositories/
    ├── domain/
    │   ├── models/
    │   └── services/
    ├── presentation/
    │   ├── screens/
    │   ├── widgets/
    │   └── providers/
    └── sync/
        ├── sync_manager.dart
        └── conflict_resolver.dart
    ```

#### Day 5-6: CI/CD Setup

- [ ] Setup GitHub Actions workflow:
    ```yaml
    # .github/workflows/backend.yml
    name: Backend CI
    on: [push, pull_request]
    jobs:
        test:
            runs-on: ubuntu-latest
            services:
                postgres:
                    image: postgres:15
                    env:
                        POSTGRES_PASSWORD: test
                    options: --health-cmd pg_isready
            steps:
                - uses: actions/checkout@v4
                - uses: actions/setup-python@v5
                  with:
                      python-version: "3.11"
                - run: pip install poetry && poetry install
                - run: poetry run ruff check app/
                - run: poetry run pytest --cov=app
    ```
- [ ] Setup Flutter CI workflow
- [ ] Setup auto-deploy ke staging (optional)

#### Day 6-7: Documentation & Handoff

- [ ] Buat API contract documentation (OpenAPI)
- [ ] Buat environment setup guide untuk tim
- [ ] Brief Attila tentang API structure
- [ ] Review wireframe dari Wulan

### Deliverables Minggu 1

| Deliverable                        | Format         | Status |
| ---------------------------------- | -------------- | ------ |
| Repository dengan struktur lengkap | Git repo       | ☐      |
| Database schema + migrations       | SQL + Alembic  | ☐      |
| Backend project skeleton           | Python/FastAPI | ☐      |
| Flutter project skeleton           | Dart/Flutter   | ☐      |
| Docker Compose setup               | YAML           | ☐      |
| CI/CD pipeline                     | GitHub Actions | ☐      |
| Environment documentation          | Markdown       | ☐      |

---

## Fase 1: MVP Foundation (Minggu 2-6)

### Minggu 2: Authentication & User Management

**Periode:** 25 Februari - 4 Maret 2026

#### Backend Tasks

| Task                                | Priority | Estimasi |
| ----------------------------------- | -------- | -------- |
| JWT authentication service          | P0       | 4 jam    |
| Login endpoint (`POST /auth/login`) | P0       | 2 jam    |
| Token refresh endpoint              | P0       | 2 jam    |
| Get current user endpoint           | P0       | 1 jam    |
| Password hashing (Argon2)           | P0       | 1 jam    |
| Role-based middleware               | P0       | 3 jam    |
| User CRUD endpoints                 | P0       | 4 jam    |
| Unit tests untuk auth               | P0       | 3 jam    |

#### Endpoint Specifications

```python
# POST /auth/login
Request:
{
    "username": "string",
    "password": "string"
}

Response:
{
    "success": true,
    "data": {
        "access_token": "string",
        "refresh_token": "string",
        "token_type": "bearer",
        "expires_in": 1800,
        "user": {
            "id": 1,
            "username": "kasir01",
            "full_name": "Kasir Satu",
            "role": "KASIR",
            "outlet_id": 1
        }
    }
}

# POST /auth/refresh
Request:
{
    "refresh_token": "string"
}

# GET /auth/me
Headers: Authorization: Bearer <token>
Response: User object

# GET /users
# POST /users
# PUT /users/{id}
# DELETE /users/{id}
```

#### Code Implementation Guide

```python
# app/services/auth_service.py
from datetime import datetime, timedelta
from jose import jwt, JWTError
from passlib.context import CryptContext
from app.config import settings

pwd_context = CryptContext(schemes=["argon2"], deprecated="auto")

class AuthService:
    def __init__(self, db: Session):
        self.db = db

    def verify_password(self, plain: str, hashed: str) -> bool:
        return pwd_context.verify(plain, hashed)

    def hash_password(self, password: str) -> str:
        return pwd_context.hash(password)

    def create_access_token(self, user_id: int, role: str) -> str:
        expire = datetime.utcnow() + timedelta(minutes=30)
        payload = {
            "sub": str(user_id),
            "role": role,
            "exp": expire,
            "type": "access"
        }
        return jwt.encode(payload, settings.SECRET_KEY, algorithm="HS256")

    def create_refresh_token(self, user_id: int) -> str:
        expire = datetime.utcnow() + timedelta(days=7)
        payload = {
            "sub": str(user_id),
            "exp": expire,
            "type": "refresh"
        }
        return jwt.encode(payload, settings.SECRET_KEY, algorithm="HS256")

    async def authenticate(self, username: str, password: str) -> User | None:
        user = await self.db.query(User).filter(User.username == username).first()
        if not user or not self.verify_password(password, user.password_hash):
            return None
        return user
```

### Minggu 3: Product & Transaction API

**Periode:** 4-11 Maret 2026

#### Backend Tasks

| Task                        | Priority | Estimasi |
| --------------------------- | -------- | -------- |
| Category CRUD endpoints     | P0       | 3 jam    |
| Product CRUD endpoints      | P0       | 4 jam    |
| Barcode lookup endpoint     | P0       | 2 jam    |
| Stock check endpoint        | P0       | 2 jam    |
| Shift management endpoints  | P0       | 4 jam    |
| Transaction create endpoint | P0       | 6 jam    |
| Transaction void endpoint   | P0       | 3 jam    |
| Receipt generation          | P0       | 4 jam    |
| Stock auto-deduction logic  | P0       | 3 jam    |
| Unit tests                  | P0       | 4 jam    |

#### Endpoint Specifications

```python
# Products
GET    /products              # List with pagination
GET    /products/{id}         # Detail
GET    /products/barcode/{barcode}  # Lookup by barcode
POST   /products              # Create (Admin only)
PUT    /products/{id}         # Update
DELETE /products/{id}         # Soft delete

# Categories
GET    /categories
POST   /categories
PUT    /categories/{id}
DELETE /categories/{id}

# Shifts
POST   /shifts/open           # Open shift with start_cash
POST   /shifts/close          # Close shift
GET    /shifts/current        # Get active shift
GET    /shifts/{id}           # Shift detail with transactions

# Transactions
POST   /transactions          # Create transaction
GET    /transactions/{id}     # Detail
POST   /transactions/{id}/void  # Void (with reason)
GET    /transactions/{id}/receipt  # Get receipt data
```

#### Transaction Service Logic

```python
# app/services/transaction_service.py
class TransactionService:
    async def create_transaction(
        self,
        data: TransactionCreate,
        user_id: int,
        shift_id: int
    ) -> Transaction:
        # 1. Validate shift is open
        shift = await self.validate_shift(shift_id)

        # 2. Validate all items
        validated_items = []
        for item in data.items:
            product = await self.get_product(item.product_id)

            # Check stock
            if product.current_stock < item.quantity:
                raise InsufficientStockError(
                    product_id=item.product_id,
                    available=product.current_stock,
                    requested=item.quantity
                )

            validated_items.append({
                "product": product,
                "quantity": item.quantity,
                "unit_price": product.unit_price,
                "subtotal": product.unit_price * item.quantity
            })

        # 3. Calculate totals
        subtotal = sum(item["subtotal"] for item in validated_items)
        tax_amount = subtotal * Decimal("0.11")  # PPN 11%
        total_amount = subtotal + tax_amount - data.discount_amount

        # 4. Create transaction (atomic)
        async with self.db.begin():
            transaction = Transaction(
                transaction_code=self.generate_code(),
                user_id=user_id,
                shift_id=shift_id,
                outlet_id=shift.outlet_id,
                subtotal=subtotal,
                tax_amount=tax_amount,
                discount_amount=data.discount_amount,
                total_amount=total_amount,
                payment_method=data.payment_method,
                status="COMPLETED"
            )
            self.db.add(transaction)
            await self.db.flush()

            # 5. Create items and deduct stock
            for item in validated_items:
                tx_item = TransactionItem(
                    transaction_id=transaction.id,
                    product_id=item["product"].id,
                    quantity=item["quantity"],
                    unit_price=item["unit_price"],
                    subtotal=item["subtotal"]
                )
                self.db.add(tx_item)

                # Deduct stock
                item["product"].current_stock -= item["quantity"]

                # Log stock movement
                movement = StockMovement(
                    product_id=item["product"].id,
                    movement_type="SALE",
                    quantity=-item["quantity"],
                    reference_id=transaction.id
                )
                self.db.add(movement)

            await self.db.commit()

        return transaction
```

### Minggu 4-5: Flutter Mobile Development

**Periode:** 11-25 Maret 2026

#### Mobile Tasks

| Task                         | Priority | Estimasi |
| ---------------------------- | -------- | -------- |
| API client setup (Dio)       | P0       | 3 jam    |
| Auth provider & storage      | P0       | 4 jam    |
| Login screen                 | P0       | 3 jam    |
| Open shift screen            | P0       | 4 jam    |
| Product search screen        | P0       | 6 jam    |
| Barcode scanner integration  | P0       | 4 jam    |
| Cart management              | P0       | 6 jam    |
| Checkout screen              | P0       | 6 jam    |
| Receipt screen               | P0       | 4 jam    |
| Close shift screen           | P0       | 4 jam    |
| Local database (Drift) setup | P0       | 6 jam    |

#### Screen Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                      MOBILE APP FLOW                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  [Splash] → [Login] → [Open Shift] → [Home/POS]                │
│                              │             │                    │
│                              │             ├── [Product Search] │
│                              │             ├── [Cart]           │
│                              │             ├── [Checkout]       │
│                              │             │      └── [Receipt] │
│                              │             └── [Close Shift]    │
│                              │                      │           │
│                              └──────────────────────┘           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Drift Database Schema

```dart
// lib/data/local/database.dart
import 'package:drift/drift.dart';

class Products extends Table {
  IntColumn get id => integer().autoIncrement()();
  TextColumn get sku => text()();
  TextColumn get barcode => text().nullable()();
  TextColumn get name => text()();
  IntColumn get categoryId => integer().nullable()();
  RealColumn get unitPrice => real()();
  IntColumn get currentStock => integer()();
  BoolColumn get isSynced => boolean().withDefault(const Constant(true))();
  DateTimeColumn get updatedAt => dateTime()();
}

class LocalTransactions extends Table {
  TextColumn get localId => text()();  // UUID generated locally
  IntColumn get serverId => integer().nullable()();  // Null until synced
  IntColumn get userId => integer()();
  IntColumn get shiftId => integer()();
  RealColumn get totalAmount => real()();
  TextColumn get paymentMethod => text()();
  TextColumn get status => text()();  // PENDING_SYNC, SYNCED, FAILED
  TextColumn get itemsJson => text()();  // JSON string of items
  DateTimeColumn get createdAt => dateTime()();
  DateTimeColumn get syncedAt => dateTime().nullable()();
}

class SyncQueue extends Table {
  IntColumn get id => integer().autoIncrement()();
  TextColumn get entityType => text()();  // transaction, product, etc.
  TextColumn get operation => text()();  // create, update, delete
  TextColumn get payload => text()();  // JSON
  TextColumn get status => text()();  // pending, processing, done, failed
  IntColumn get retryCount => integer().withDefault(const Constant(0))();
  DateTimeColumn get createdAt => dateTime()();
}
```

### Minggu 6: Offline Sync Mechanism

**Periode:** 14-21 Maret 2026

#### Sync Tasks

| Task                          | Priority | Estimasi |
| ----------------------------- | -------- | -------- |
| Sync manager service          | P0       | 8 jam    |
| Conflict resolution logic     | P0       | 6 jam    |
| Background sync (WorkManager) | P0       | 4 jam    |
| Connectivity monitoring       | P0       | 3 jam    |
| Sync status UI indicators     | P0       | 3 jam    |
| Backend sync endpoints        | P0       | 6 jam    |
| Integration testing           | P0       | 6 jam    |

#### Sync Manager Implementation

```dart
// lib/sync/sync_manager.dart
class SyncManager {
  final ApiClient _api;
  final AppDatabase _db;
  final ConnectivityService _connectivity;

  StreamSubscription? _connectivitySub;
  bool _isSyncing = false;

  Future<void> initialize() async {
    _connectivitySub = _connectivity.onStatusChange.listen((status) {
      if (status == ConnectivityStatus.online) {
        syncPendingData();
      }
    });
  }

  Future<SyncResult> syncPendingData() async {
    if (_isSyncing) return SyncResult.alreadySyncing();
    _isSyncing = true;

    try {
      // 1. Upload pending transactions
      final pendingTx = await _db.getPendingTransactions();
      final uploadResult = await _uploadTransactions(pendingTx);

      // 2. Download updates
      final lastSync = await _getLastSyncTimestamp();
      final updates = await _api.sync.download(since: lastSync);

      // 3. Apply updates locally
      await _applyUpdates(updates);

      // 4. Update sync timestamp
      await _setLastSyncTimestamp(DateTime.now());

      return SyncResult.success(
        uploaded: uploadResult.count,
        downloaded: updates.totalRecords,
        conflicts: uploadResult.conflicts,
      );
    } catch (e) {
      return SyncResult.error(e.toString());
    } finally {
      _isSyncing = false;
    }
  }

  Future<UploadResult> _uploadTransactions(List<LocalTransaction> txList) async {
    final results = <TransactionSyncResult>[];

    for (final tx in txList) {
      try {
        final serverTx = await _api.transactions.create(
          TransactionRequest.fromLocal(tx),
        );

        await _db.markTransactionSynced(tx.localId, serverTx.id);
        results.add(TransactionSyncResult.success(tx.localId));
      } on ApiException catch (e) {
        if (e.code == 'STOCK_INSUFFICIENT') {
          // Handle conflict
          await _db.markTransactionFailed(tx.localId, e.message);
          results.add(TransactionSyncResult.conflict(tx.localId, e.message));
        } else {
          results.add(TransactionSyncResult.error(tx.localId, e.message));
        }
      }
    }

    return UploadResult(results);
  }
}
```

#### Backend Sync Endpoints

```python
# POST /sync/upload
Request:
{
    "device_id": "uuid",
    "transactions": [
        {
            "local_id": "uuid",
            "items": [...],
            "payment_method": "CASH",
            "total_amount": 150000,
            "created_at": "2026-03-15T10:30:00Z"
        }
    ]
}

Response:
{
    "success": true,
    "data": {
        "synced": [
            {"local_id": "uuid1", "server_id": 1001}
        ],
        "conflicts": [
            {
                "local_id": "uuid2",
                "reason": "STOCK_INSUFFICIENT",
                "product_id": 5,
                "available": 2,
                "requested": 5
            }
        ]
    }
}

# GET /sync/download?since=2026-03-15T00:00:00Z
Response:
{
    "success": true,
    "data": {
        "products": [...],  // Updated/new products
        "categories": [...],
        "sync_timestamp": "2026-03-15T12:00:00Z"
    }
}
```

### Deliverables Fase 1

| Deliverable                   | Status |
| ----------------------------- | ------ |
| Auth API (login, refresh, me) | ☐      |
| User CRUD API                 | ☐      |
| Product CRUD API              | ☐      |
| Category CRUD API             | ☐      |
| Shift management API          | ☐      |
| Transaction API               | ☐      |
| Stock deduction logic         | ☐      |
| Receipt generation            | ☐      |
| Flutter app - all screens     | ☐      |
| Offline database              | ☐      |
| Sync mechanism                | ☐      |
| Unit tests (>80%)             | ☐      |
| Integration tests             | ☐      |

---

## Fase 2: Intelligence Layer (Minggu 7-12)

### Minggu 7-8: Payment Gateway

**Periode:** 21 Maret - 4 April 2026

#### Tasks

| Task                      | Priority | Estimasi |
| ------------------------- | -------- | -------- |
| Midtrans/Xendit SDK setup | P0       | 3 jam    |
| QRIS generation endpoint  | P0       | 4 jam    |
| Payment webhook handler   | P0       | 6 jam    |
| Payment status checking   | P0       | 3 jam    |
| Signature verification    | P0       | 3 jam    |
| Flutter QRIS screen       | P0       | 6 jam    |
| Payment polling           | P0       | 4 jam    |
| Reconciliation job        | P1       | 4 jam    |

#### Payment Service Implementation

```python
# app/services/payment_service.py
import httpx
import base64
import hashlib
from app.config import settings

class MidtransService:
    def __init__(self):
        self.server_key = settings.MIDTRANS_SERVER_KEY
        self.base_url = (
            "https://api.midtrans.com"
            if settings.ENVIRONMENT == "production"
            else "https://api.sandbox.midtrans.com"
        )
        self.auth = base64.b64encode(
            f"{self.server_key}:".encode()
        ).decode()

    async def create_qris(
        self,
        order_id: str,
        amount: int,
        customer_name: str
    ) -> dict:
        async with httpx.AsyncClient() as client:
            response = await client.post(
                f"{self.base_url}/v2/charge",
                headers={
                    "Authorization": f"Basic {self.auth}",
                    "Content-Type": "application/json"
                },
                json={
                    "payment_type": "qris",
                    "transaction_details": {
                        "order_id": order_id,
                        "gross_amount": amount
                    },
                    "qris": {
                        "acquirer": "gopay"
                    },
                    "custom_expiry": {
                        "expiry_duration": 15,
                        "unit": "minute"
                    }
                }
            )
            return response.json()

    def verify_signature(self, payload: dict) -> bool:
        """Verify webhook signature from Midtrans"""
        order_id = payload.get("order_id")
        status_code = payload.get("status_code")
        gross_amount = payload.get("gross_amount")
        signature = payload.get("signature_key")

        raw = f"{order_id}{status_code}{gross_amount}{self.server_key}"
        expected = hashlib.sha512(raw.encode()).hexdigest()

        return signature == expected
```

### Minggu 9-10: Rule-Based Fraud Detection

**Periode:** 4-18 April 2026

#### Tasks

| Task                         | Priority | Estimasi |
| ---------------------------- | -------- | -------- |
| Fraud rules engine           | P0       | 8 jam    |
| Alert model & API            | P0       | 4 jam    |
| Unusual hour detection       | P0       | 2 jam    |
| High void rate detection     | P0       | 3 jam    |
| Excessive discount detection | P0       | 2 jam    |
| Velocity check               | P0       | 3 jam    |
| Alert notification system    | P1       | 4 jam    |

#### Fraud Detection Rules

```python
# app/services/fraud_service.py
from dataclasses import dataclass
from typing import List, Callable
from decimal import Decimal

@dataclass
class RuleResult:
    triggered: bool
    rule_name: str
    description: str = ""
    weight: float = 0.0

class FraudDetectionService:
    def __init__(self, db: Session):
        self.db = db
        self.rules: List[Callable] = [
            self._check_unusual_hour,
            self._check_void_rate,
            self._check_excessive_discount,
            self._check_velocity,
            self._check_manual_price,
        ]

    async def evaluate(
        self,
        transaction: dict,
        context: dict
    ) -> FraudResult:
        """Evaluate transaction against all rules"""
        results = []
        total_score = 0.0

        for rule in self.rules:
            result = await rule(transaction, context)
            if result.triggered:
                results.append(result)
                total_score += result.weight

        # Determine action based on score
        if total_score >= 0.8:
            action = "BLOCK"
        elif total_score >= 0.5:
            action = "FLAG"
        else:
            action = "APPROVE"

        return FraudResult(
            score=min(total_score, 1.0),
            action=action,
            factors=results
        )

    async def _check_unusual_hour(self, tx, ctx) -> RuleResult:
        hour = tx["created_at"].hour
        # Business hours: 6 AM - 11 PM
        if hour < 6 or hour > 23:
            return RuleResult(
                triggered=True,
                rule_name="unusual_hour",
                description=f"Transaksi jam {hour}:00 di luar jam operasional",
                weight=0.3
            )
        return RuleResult(triggered=False, rule_name="unusual_hour")

    async def _check_void_rate(self, tx, ctx) -> RuleResult:
        shift_id = tx["shift_id"]
        stats = await self._get_shift_stats(shift_id)
        void_rate = stats["void_count"] / max(stats["total_count"], 1)

        if void_rate > 0.10:  # >10%
            return RuleResult(
                triggered=True,
                rule_name="high_void_rate",
                description=f"Void rate shift: {void_rate*100:.1f}%",
                weight=0.4
            )
        return RuleResult(triggered=False, rule_name="high_void_rate")

    async def _check_excessive_discount(self, tx, ctx) -> RuleResult:
        discount_percent = tx["discount_amount"] / tx["subtotal"] * 100
        if discount_percent > 30:
            return RuleResult(
                triggered=True,
                rule_name="excessive_discount",
                description=f"Diskon {discount_percent:.1f}% melebihi batas",
                weight=0.35
            )
        return RuleResult(triggered=False, rule_name="excessive_discount")

    async def _check_velocity(self, tx, ctx) -> RuleResult:
        # Check transactions in last 5 minutes
        recent_count = await self._count_recent_transactions(
            user_id=tx["user_id"],
            minutes=5
        )
        if recent_count > 10:  # >10 tx in 5 min is suspicious
            return RuleResult(
                triggered=True,
                rule_name="high_velocity",
                description=f"{recent_count} transaksi dalam 5 menit",
                weight=0.25
            )
        return RuleResult(triggered=False, rule_name="high_velocity")
```

### Minggu 11-12: Data Pipeline & ML Prep

**Periode:** 18 April - 2 Mei 2026

#### Tasks

| Task                     | Priority | Estimasi |
| ------------------------ | -------- | -------- |
| Data aggregation jobs    | P0       | 6 jam    |
| Feature engineering      | P0       | 8 jam    |
| ML training data export  | P0       | 4 jam    |
| Prophet model prototype  | P1       | 6 jam    |
| QRIS Flutter integration | P0       | 6 jam    |
| Bug fixes                | P0       | 8 jam    |

---

## Fase 3: Security & AI (Minggu 13-16)

### Minggu 13-14: Blockchain & ML Training

**Periode:** 2-16 Mei 2026

#### Tasks

| Task                         | Priority | Estimasi |
| ---------------------------- | -------- | -------- |
| Blockchain hashing service   | P0       | 8 jam    |
| Chain integrity verification | P0       | 4 jam    |
| Prophet model training       | P0       | 8 jam    |
| Isolation Forest training    | P0       | 6 jam    |
| MLflow setup                 | P0       | 4 jam    |

#### Blockchain Service

```python
# app/services/blockchain_service.py
import hashlib
import json
from datetime import datetime

class BlockchainService:
    GENESIS_HASH = "0" * 64

    def __init__(self, db: Session):
        self.db = db

    def calculate_hash(
        self,
        data: dict,
        previous_hash: str,
        timestamp: datetime
    ) -> str:
        block = {
            "data": data,
            "previous_hash": previous_hash,
            "timestamp": timestamp.isoformat()
        }
        block_string = json.dumps(block, sort_keys=True, default=str)
        return hashlib.sha256(block_string.encode()).hexdigest()

    async def add_transaction(self, transaction) -> BlockchainLedger:
        # Get previous block
        last = await self.db.query(BlockchainLedger)\
            .order_by(BlockchainLedger.id.desc())\
            .first()

        previous_hash = last.current_hash if last else self.GENESIS_HASH

        # Create data snapshot
        snapshot = {
            "tx_id": transaction.id,
            "tx_code": transaction.transaction_code,
            "amount": str(transaction.total_amount),
            "items": [
                {"pid": i.product_id, "qty": i.quantity}
                for i in transaction.items
            ],
            "user_id": transaction.user_id
        }

        current_hash = self.calculate_hash(
            snapshot,
            previous_hash,
            transaction.created_at
        )

        entry = BlockchainLedger(
            transaction_id=transaction.id,
            previous_hash=previous_hash,
            current_hash=current_hash,
            data_snapshot=snapshot
        )

        self.db.add(entry)
        await self.db.commit()
        return entry

    async def verify_chain(
        self,
        start_id: int = None,
        end_id: int = None
    ) -> VerificationResult:
        query = self.db.query(BlockchainLedger).order_by(BlockchainLedger.id)

        if start_id:
            query = query.filter(BlockchainLedger.id >= start_id)
        if end_id:
            query = query.filter(BlockchainLedger.id <= end_id)

        blocks = await query.all()
        invalid = []

        for i, block in enumerate(blocks):
            # Verify hash
            expected = self.calculate_hash(
                block.data_snapshot,
                block.previous_hash,
                # Need original timestamp from transaction
            )

            if block.current_hash != expected:
                invalid.append({
                    "id": block.id,
                    "reason": "Hash mismatch"
                })

            # Verify chain link
            if i > 0 and block.previous_hash != blocks[i-1].current_hash:
                invalid.append({
                    "id": block.id,
                    "reason": "Chain broken"
                })

        return VerificationResult(
            valid=len(invalid) == 0,
            checked=len(blocks),
            invalid=invalid
        )
```

#### ML Training Pipeline

```python
# ml/training/inventory_forecast.py
from prophet import Prophet
import pandas as pd
import mlflow
import joblib

class InventoryForecaster:
    def __init__(self):
        self.model = None

    def train(self, df: pd.DataFrame, product_id: int):
        """
        df should have columns: ds (date), y (quantity sold)
        """
        with mlflow.start_run(run_name=f"prophet_product_{product_id}"):
            # Add Indonesian holidays
            self.model = Prophet(
                yearly_seasonality=True,
                weekly_seasonality=True,
                daily_seasonality=False,
            )
            self.model.add_country_holidays(country_name='ID')

            self.model.fit(df)

            # Log params
            mlflow.log_param("product_id", product_id)
            mlflow.log_param("training_rows", len(df))

            # Log model
            mlflow.prophet.log_model(self.model, "model")

            return self

    def predict(self, periods: int = 30) -> pd.DataFrame:
        future = self.model.make_future_dataframe(periods=periods)
        return self.model.predict(future)

    def save(self, path: str):
        joblib.dump(self.model, path)

    @classmethod
    def load(cls, path: str):
        instance = cls()
        instance.model = joblib.load(path)
        return instance
```

### Minggu 15-16: ML Deployment & Launch

**Periode:** 16-30 Mei 2026

#### Tasks

| Task                  | Priority | Estimasi |
| --------------------- | -------- | -------- |
| ML serving endpoints  | P0       | 6 jam    |
| Stock prediction API  | P0       | 4 jam    |
| Fraud ML integration  | P0       | 6 jam    |
| WhatsApp notification | P0       | 6 jam    |
| Security hardening    | P0       | 6 jam    |
| Load testing          | P0       | 4 jam    |
| Final bug fixes       | P0       | 8 jam    |

---

## Testing Checklist

### Unit Tests

| Module              | Coverage Target |
| ------------------- | --------------- |
| Auth service        | >90%            |
| Transaction service | >90%            |
| Inventory service   | >85%            |
| Fraud detection     | >85%            |
| Blockchain service  | >90%            |
| Payment service     | >85%            |

### Integration Tests

- [ ] Login flow end-to-end
- [ ] Transaction creation with stock deduction
- [ ] Offline transaction + sync
- [ ] QRIS payment flow
- [ ] Fraud detection triggering
- [ ] Blockchain hash verification

### Load Tests

| Scenario                | Target     |
| ----------------------- | ---------- |
| Concurrent transactions | 100/second |
| API response time       | <500ms p95 |
| Database query time     | <100ms p95 |

---

## Tools & Resources

### Development Tools

| Tool           | Purpose             |
| -------------- | ------------------- |
| VS Code        | IDE                 |
| Postman        | API testing         |
| DBeaver        | Database management |
| Android Studio | Flutter development |
| Docker Desktop | Local services      |

### Libraries Reference

| Library    | Documentation                      |
| ---------- | ---------------------------------- |
| FastAPI    | https://fastapi.tiangolo.com       |
| SQLAlchemy | https://docs.sqlalchemy.org        |
| Flutter    | https://docs.flutter.dev           |
| Drift      | https://drift.simonbinder.eu       |
| Prophet    | https://facebook.github.io/prophet |

---

## Weekly Review Checklist

Setiap akhir minggu, Van harus review:

- [ ] Semua tasks minggu ini complete
- [ ] Unit tests passing
- [ ] No critical bugs
- [ ] API documentation updated
- [ ] Kode sudah di-review/merge ke develop
- [ ] Next week tasks jelas
- [ ] Blockers identified dan escalated

---

_Last updated: 7 Februari 2026_
