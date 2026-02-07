# Planning Attila - Frontend Developer

## Dokumen Perencanaan Detail untuk Attila

**Role:** Frontend Developer (Dashboard Owner)  
**Periode:** 18 Februari - 10 Juni 2026 (16 Minggu)

---

## Ringkasan Tanggung Jawab

| Area                   | Deskripsi                                        |
| ---------------------- | ------------------------------------------------ |
| **Dashboard Owner**    | Development dashboard web untuk pemilik usaha    |
| **React Development**  | Component development, state management, routing |
| **API Integration**    | Integrasi dengan backend API                     |
| **Data Visualization** | Charts, graphs, dan reporting                    |
| **Mentoring**          | Membimbing Yoga dalam frontend development       |

---

## Tech Stack yang Digunakan

| Layer            | Teknologi             | Versi  |
| ---------------- | --------------------- | ------ |
| Framework        | React                 | 18+    |
| Language         | TypeScript            | 5+     |
| Build Tool       | Vite                  | 5+     |
| Styling          | Tailwind CSS          | 3+     |
| UI Components    | shadcn/ui             | Latest |
| State Management | Zustand               | 4+     |
| API State        | TanStack Query        | 5+     |
| Routing          | React Router          | 6+     |
| Charts           | Recharts              | 2+     |
| Forms            | React Hook Form + Zod | Latest |
| HTTP Client      | Axios                 | 1+     |

---

## Fase 0: Setup (Minggu 1)

**Periode:** 18-25 Februari 2026

### Checklist Tasks

#### Day 1-2: Project Initialization

- [ ] Initialize React project dengan Vite:

    ```bash
    cd apps/dashboard
    npm create vite@latest . -- --template react-ts
    npm install
    ```

- [ ] Install core dependencies:

    ```bash
    npm install @tanstack/react-query axios zustand react-router-dom
    npm install tailwindcss postcss autoprefixer
    npm install -D @types/node
    npx tailwindcss init -p
    ```

- [ ] Install UI dependencies:

    ```bash
    npx shadcn-ui@latest init
    npx shadcn-ui@latest add button card input label table tabs
    npx shadcn-ui@latest add dialog dropdown-menu select toast
    npx shadcn-ui@latest add badge avatar separator skeleton
    ```

- [ ] Install form & validation:

    ```bash
    npm install react-hook-form @hookform/resolvers zod
    ```

- [ ] Install charts & utilities:
    ```bash
    npm install recharts date-fns lucide-react
    npm install @tanstack/react-table
    ```

#### Day 2-3: Project Structure Setup

- [ ] Setup folder structure:
    ```
    apps/dashboard/src/
    ├── main.tsx
    ├── App.tsx
    ├── index.css
    ├── components/
    │   ├── ui/           # shadcn components
    │   ├── layout/
    │   │   ├── Sidebar.tsx
    │   │   ├── Header.tsx
    │   │   ├── MainLayout.tsx
    │   │   └── AuthLayout.tsx
    │   ├── common/
    │   │   ├── DataTable.tsx
    │   │   ├── PageHeader.tsx
    │   │   ├── LoadingSpinner.tsx
    │   │   ├── EmptyState.tsx
    │   │   └── ErrorBoundary.tsx
    │   └── features/
    │       ├── auth/
    │       ├── products/
    │       ├── transactions/
    │       ├── reports/
    │       ├── employees/
    │       └── settings/
    ├── pages/
    │   ├── LoginPage.tsx
    │   ├── DashboardPage.tsx
    │   ├── ProductsPage.tsx
    │   ├── TransactionsPage.tsx
    │   ├── ReportsPage.tsx
    │   ├── EmployeesPage.tsx
    │   ├── AlertsPage.tsx
    │   └── SettingsPage.tsx
    ├── hooks/
    │   ├── useAuth.ts
    │   ├── useProducts.ts
    │   ├── useTransactions.ts
    │   └── useReports.ts
    ├── services/
    │   ├── api.ts
    │   ├── auth.service.ts
    │   ├── products.service.ts
    │   ├── transactions.service.ts
    │   └── reports.service.ts
    ├── stores/
    │   ├── authStore.ts
    │   └── uiStore.ts
    ├── types/
    │   ├── api.types.ts
    │   ├── product.types.ts
    │   ├── transaction.types.ts
    │   └── user.types.ts
    ├── utils/
    │   ├── formatters.ts
    │   ├── validators.ts
    │   └── constants.ts
    └── lib/
        └── utils.ts
    ```

#### Day 3-4: Core Setup

- [ ] Setup API client:

    ```typescript
    // src/services/api.ts
    import axios from "axios";

    const api = axios.create({
        baseURL: import.meta.env.VITE_API_URL || "http://localhost:8000/api/v1",
        headers: {
            "Content-Type": "application/json"
        }
    });

    // Request interceptor - add token
    api.interceptors.request.use((config) => {
        const token = localStorage.getItem("access_token");
        if (token) {
            config.headers.Authorization = `Bearer ${token}`;
        }
        return config;
    });

    // Response interceptor - handle errors
    api.interceptors.response.use(
        (response) => response,
        async (error) => {
            if (error.response?.status === 401) {
                // Try refresh token
                const refreshToken = localStorage.getItem("refresh_token");
                if (refreshToken) {
                    try {
                        const { data } = await axios.post("/auth/refresh", {
                            refresh_token: refreshToken
                        });
                        localStorage.setItem("access_token", data.access_token);
                        error.config.headers.Authorization = `Bearer ${data.access_token}`;
                        return api(error.config);
                    } catch {
                        localStorage.clear();
                        window.location.href = "/login";
                    }
                }
            }
            return Promise.reject(error);
        }
    );

    export default api;
    ```

- [ ] Setup auth store:

    ```typescript
    // src/stores/authStore.ts
    import { create } from "zustand";
    import { persist } from "zustand/middleware";

    interface User {
        id: number;
        username: string;
        full_name: string;
        role: "KASIR" | "ADMIN" | "OWNER";
        outlet_id: number;
    }

    interface AuthState {
        user: User | null;
        isAuthenticated: boolean;
        setUser: (user: User) => void;
        logout: () => void;
    }

    export const useAuthStore = create<AuthState>()(
        persist(
            (set) => ({
                user: null,
                isAuthenticated: false,
                setUser: (user) => set({ user, isAuthenticated: true }),
                logout: () => {
                    localStorage.removeItem("access_token");
                    localStorage.removeItem("refresh_token");
                    set({ user: null, isAuthenticated: false });
                }
            }),
            { name: "auth-storage" }
        )
    );
    ```

- [ ] Setup routing:

    ```typescript
    // src/App.tsx
    import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
    import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
    import { useAuthStore } from './stores/authStore';

    const queryClient = new QueryClient();

    function ProtectedRoute({ children }: { children: React.ReactNode }) {
      const isAuthenticated = useAuthStore((s) => s.isAuthenticated);
      return isAuthenticated ? children : <Navigate to="/login" />;
    }

    export default function App() {
      return (
        <QueryClientProvider client={queryClient}>
          <BrowserRouter>
            <Routes>
              <Route path="/login" element={<LoginPage />} />
              <Route
                path="/"
                element={
                  <ProtectedRoute>
                    <MainLayout />
                  </ProtectedRoute>
                }
              >
                <Route index element={<DashboardPage />} />
                <Route path="products" element={<ProductsPage />} />
                <Route path="transactions" element={<TransactionsPage />} />
                <Route path="reports" element={<ReportsPage />} />
                <Route path="employees" element={<EmployeesPage />} />
                <Route path="alerts" element={<AlertsPage />} />
                <Route path="settings" element={<SettingsPage />} />
              </Route>
            </Routes>
          </BrowserRouter>
        </QueryClientProvider>
      );
    }
    ```

#### Day 4-5: Layout Components

- [ ] Create MainLayout:

    ```typescript
    // src/components/layout/MainLayout.tsx
    import { Outlet } from 'react-router-dom';
    import { Sidebar } from './Sidebar';
    import { Header } from './Header';

    export function MainLayout() {
      return (
        <div className="flex h-screen bg-gray-100">
          <Sidebar />
          <div className="flex-1 flex flex-col overflow-hidden">
            <Header />
            <main className="flex-1 overflow-y-auto p-6">
              <Outlet />
            </main>
          </div>
        </div>
      );
    }
    ```

- [ ] Create Sidebar:

    ```typescript
    // src/components/layout/Sidebar.tsx
    import { NavLink } from 'react-router-dom';
    import {
      LayoutDashboard,
      Package,
      Receipt,
      BarChart3,
      Users,
      Bell,
      Settings,
    } from 'lucide-react';

    const navItems = [
      { to: '/', icon: LayoutDashboard, label: 'Dashboard' },
      { to: '/products', icon: Package, label: 'Produk' },
      { to: '/transactions', icon: Receipt, label: 'Transaksi' },
      { to: '/reports', icon: BarChart3, label: 'Laporan' },
      { to: '/employees', icon: Users, label: 'Karyawan' },
      { to: '/alerts', icon: Bell, label: 'Alerts' },
      { to: '/settings', icon: Settings, label: 'Pengaturan' },
    ];

    export function Sidebar() {
      return (
        <aside className="w-64 bg-white shadow-md">
          <div className="p-6">
            <h1 className="text-2xl font-bold text-primary">AIPOS</h1>
          </div>
          <nav className="mt-6">
            {navItems.map((item) => (
              <NavLink
                key={item.to}
                to={item.to}
                className={({ isActive }) =>
                  `flex items-center px-6 py-3 text-gray-600 hover:bg-gray-50
                   ${isActive ? 'bg-primary/10 text-primary border-r-4 border-primary' : ''}`
                }
              >
                <item.icon className="w-5 h-5 mr-3" />
                {item.label}
              </NavLink>
            ))}
          </nav>
        </aside>
      );
    }
    ```

#### Day 5-6: Pelajari API Contract

- [ ] Review API documentation dari Van
- [ ] Buat type definitions berdasarkan API:

    ```typescript
    // src/types/api.types.ts
    export interface ApiResponse<T> {
        success: boolean;
        data: T;
        meta?: {
            page: number;
            limit: number;
            total: number;
        };
    }

    export interface ApiError {
        success: false;
        error: {
            code: string;
            message: string;
            details?: unknown;
        };
    }

    // src/types/product.types.ts
    export interface Product {
        id: number;
        sku: string;
        barcode: string | null;
        name: string;
        category_id: number | null;
        category_name?: string;
        unit_price: number;
        cost_price: number | null;
        current_stock: number;
        min_stock_level: number;
        is_active: boolean;
        created_at: string;
        updated_at: string;
    }

    export interface CreateProductDto {
        sku: string;
        barcode?: string;
        name: string;
        category_id?: number;
        unit_price: number;
        cost_price?: number;
        current_stock: number;
        min_stock_level?: number;
    }

    // src/types/transaction.types.ts
    export interface Transaction {
        id: number;
        transaction_code: string;
        user_id: number;
        user_name: string;
        outlet_id: number;
        subtotal: number;
        tax_amount: number;
        discount_amount: number;
        total_amount: number;
        payment_method: "CASH" | "QRIS" | "DEBIT";
        status: "PENDING" | "COMPLETED" | "VOIDED";
        items: TransactionItem[];
        created_at: string;
    }

    export interface TransactionItem {
        id: number;
        product_id: number;
        product_name: string;
        quantity: number;
        unit_price: number;
        subtotal: number;
    }
    ```

#### Day 6-7: Mock API & Basic Pages

- [ ] Setup mock data untuk development:

    ```typescript
    // src/mocks/products.mock.ts
    export const mockProducts: Product[] = [
        {
            id: 1,
            sku: "PRD001",
            barcode: "8991234567890",
            name: "Indomie Goreng",
            category_id: 1,
            category_name: "Makanan",
            unit_price: 3500,
            cost_price: 3000,
            current_stock: 100,
            min_stock_level: 20,
            is_active: true,
            created_at: "2026-02-01T00:00:00Z",
            updated_at: "2026-02-01T00:00:00Z"
        }
        // ... more products
    ];
    ```

- [ ] Buat halaman placeholder untuk semua routes
- [ ] Pastikan routing berjalan dengan baik

### Deliverables Minggu 1

| Deliverable                | Status |
| -------------------------- | ------ |
| React project initialized  | ☐      |
| All dependencies installed | ☐      |
| Folder structure created   | ☐      |
| API client configured      | ☐      |
| Auth store setup           | ☐      |
| Routing configured         | ☐      |
| Layout components created  | ☐      |
| Type definitions ready     | ☐      |
| All page placeholders      | ☐      |

---

## Fase 1: MVP Foundation (Minggu 2-6)

### Minggu 2-3: Auth & Basic Setup

**Periode:** 25 Februari - 11 Maret 2026

#### Tasks

| Task                    | PIC    | Priority | Estimasi |
| ----------------------- | ------ | -------- | -------- |
| Login page UI           | Attila | P0       | 4 jam    |
| Login form validation   | Attila | P0       | 2 jam    |
| Login API integration   | Attila | P0       | 3 jam    |
| Protected route logic   | Attila | P0       | 2 jam    |
| Dashboard skeleton      | Attila | P0       | 4 jam    |
| Summary cards component | Yoga   | P0       | 3 jam    |
| Setup component library | Yoga   | P0       | 4 jam    |

#### Login Page Implementation

```typescript
// src/pages/LoginPage.tsx
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { useAuthStore } from '@/stores/authStore';
import { authService } from '@/services/auth.service';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';

const loginSchema = z.object({
  username: z.string().min(1, 'Username wajib diisi'),
  password: z.string().min(1, 'Password wajib diisi'),
});

type LoginForm = z.infer<typeof loginSchema>;

export function LoginPage() {
  const navigate = useNavigate();
  const setUser = useAuthStore((s) => s.setUser);
  const [error, setError] = useState<string | null>(null);
  const [isLoading, setIsLoading] = useState(false);

  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<LoginForm>({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = async (data: LoginForm) => {
    setIsLoading(true);
    setError(null);

    try {
      const response = await authService.login(data.username, data.password);
      localStorage.setItem('access_token', response.access_token);
      localStorage.setItem('refresh_token', response.refresh_token);
      setUser(response.user);
      navigate('/');
    } catch (err: any) {
      setError(err.response?.data?.error?.message || 'Login gagal');
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100">
      <Card className="w-full max-w-md">
        <CardHeader className="text-center">
          <CardTitle className="text-2xl">AIPOS</CardTitle>
          <p className="text-gray-500">Masuk ke dashboard</p>
        </CardHeader>
        <CardContent>
          <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
            {error && (
              <div className="p-3 bg-red-50 text-red-600 rounded-md text-sm">
                {error}
              </div>
            )}

            <div className="space-y-2">
              <Label htmlFor="username">Username</Label>
              <Input
                id="username"
                {...register('username')}
                placeholder="Masukkan username"
              />
              {errors.username && (
                <p className="text-red-500 text-sm">{errors.username.message}</p>
              )}
            </div>

            <div className="space-y-2">
              <Label htmlFor="password">Password</Label>
              <Input
                id="password"
                type="password"
                {...register('password')}
                placeholder="Masukkan password"
              />
              {errors.password && (
                <p className="text-red-500 text-sm">{errors.password.message}</p>
              )}
            </div>

            <Button type="submit" className="w-full" disabled={isLoading}>
              {isLoading ? 'Memproses...' : 'Masuk'}
            </Button>
          </form>
        </CardContent>
      </Card>
    </div>
  );
}
```

#### Dashboard Page Structure

```typescript
// src/pages/DashboardPage.tsx
import { SummaryCards } from '@/components/features/dashboard/SummaryCards';
import { SalesChart } from '@/components/features/dashboard/SalesChart';
import { TopProducts } from '@/components/features/dashboard/TopProducts';
import { RecentTransactions } from '@/components/features/dashboard/RecentTransactions';
import { LowStockAlerts } from '@/components/features/dashboard/LowStockAlerts';

export function DashboardPage() {
  return (
    <div className="space-y-6">
      <h1 className="text-2xl font-bold">Dashboard</h1>

      {/* Summary Cards */}
      <SummaryCards />

      {/* Charts Row */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <SalesChart />
        <TopProducts />
      </div>

      {/* Bottom Row */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <RecentTransactions />
        <LowStockAlerts />
      </div>
    </div>
  );
}
```

### Minggu 4-5: Product & Employee Management

**Periode:** 11-25 Maret 2026

#### Tasks

| Task                    | PIC    | Priority | Estimasi |
| ----------------------- | ------ | -------- | -------- |
| Product list page       | Attila | P0       | 6 jam    |
| Product table component | Attila | P0       | 4 jam    |
| Product CRUD modal      | Attila | P0       | 6 jam    |
| Product search & filter | Attila | P0       | 4 jam    |
| Employee list page      | Yoga   | P0       | 6 jam    |
| Employee CRUD modal     | Yoga   | P0       | 6 jam    |

#### Products Page Implementation

```typescript
// src/pages/ProductsPage.tsx
import { useState } from 'react';
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { Plus, Search, Edit, Trash2 } from 'lucide-react';
import { productsService } from '@/services/products.service';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { DataTable } from '@/components/common/DataTable';
import { ProductModal } from '@/components/features/products/ProductModal';
import { formatCurrency } from '@/utils/formatters';
import type { Product } from '@/types/product.types';

export function ProductsPage() {
  const queryClient = useQueryClient();
  const [search, setSearch] = useState('');
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [selectedProduct, setSelectedProduct] = useState<Product | null>(null);

  const { data, isLoading } = useQuery({
    queryKey: ['products', search],
    queryFn: () => productsService.getAll({ search }),
  });

  const deleteMutation = useMutation({
    mutationFn: productsService.delete,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['products'] });
    },
  });

  const columns = [
    { header: 'SKU', accessor: 'sku' },
    { header: 'Nama Produk', accessor: 'name' },
    { header: 'Kategori', accessor: 'category_name' },
    {
      header: 'Harga',
      accessor: 'unit_price',
      cell: (value: number) => formatCurrency(value),
    },
    {
      header: 'Stok',
      accessor: 'current_stock',
      cell: (value: number, row: Product) => (
        <span
          className={
            value <= row.min_stock_level ? 'text-red-600 font-medium' : ''
          }
        >
          {value}
        </span>
      ),
    },
    {
      header: 'Aksi',
      accessor: 'id',
      cell: (_: number, row: Product) => (
        <div className="flex gap-2">
          <Button
            variant="ghost"
            size="sm"
            onClick={() => {
              setSelectedProduct(row);
              setIsModalOpen(true);
            }}
          >
            <Edit className="w-4 h-4" />
          </Button>
          <Button
            variant="ghost"
            size="sm"
            onClick={() => {
              if (confirm('Hapus produk ini?')) {
                deleteMutation.mutate(row.id);
              }
            }}
          >
            <Trash2 className="w-4 h-4 text-red-500" />
          </Button>
        </div>
      ),
    },
  ];

  return (
    <div className="space-y-6">
      <div className="flex justify-between items-center">
        <h1 className="text-2xl font-bold">Produk</h1>
        <Button onClick={() => {
          setSelectedProduct(null);
          setIsModalOpen(true);
        }}>
          <Plus className="w-4 h-4 mr-2" />
          Tambah Produk
        </Button>
      </div>

      <div className="flex gap-4">
        <div className="relative flex-1 max-w-sm">
          <Search className="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" />
          <Input
            placeholder="Cari produk..."
            value={search}
            onChange={(e) => setSearch(e.target.value)}
            className="pl-10"
          />
        </div>
      </div>

      <DataTable
        columns={columns}
        data={data?.data || []}
        isLoading={isLoading}
      />

      <ProductModal
        open={isModalOpen}
        onOpenChange={setIsModalOpen}
        product={selectedProduct}
      />
    </div>
  );
}
```

#### DataTable Component (Reusable)

```typescript
// src/components/common/DataTable.tsx
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table';
import { Skeleton } from '@/components/ui/skeleton';

interface Column<T> {
  header: string;
  accessor: keyof T | string;
  cell?: (value: any, row: T) => React.ReactNode;
}

interface DataTableProps<T> {
  columns: Column<T>[];
  data: T[];
  isLoading?: boolean;
}

export function DataTable<T extends Record<string, any>>({
  columns,
  data,
  isLoading,
}: DataTableProps<T>) {
  if (isLoading) {
    return (
      <div className="space-y-3">
        {[...Array(5)].map((_, i) => (
          <Skeleton key={i} className="h-12 w-full" />
        ))}
      </div>
    );
  }

  return (
    <div className="border rounded-lg">
      <Table>
        <TableHeader>
          <TableRow>
            {columns.map((col) => (
              <TableHead key={col.header}>{col.header}</TableHead>
            ))}
          </TableRow>
        </TableHeader>
        <TableBody>
          {data.length === 0 ? (
            <TableRow>
              <TableCell colSpan={columns.length} className="text-center py-8">
                Tidak ada data
              </TableCell>
            </TableRow>
          ) : (
            data.map((row, i) => (
              <TableRow key={i}>
                {columns.map((col) => {
                  const value = row[col.accessor as keyof T];
                  return (
                    <TableCell key={col.header}>
                      {col.cell ? col.cell(value, row) : String(value ?? '-')}
                    </TableCell>
                  );
                })}
              </TableRow>
            ))
          )}
        </TableBody>
      </Table>
    </div>
  );
}
```

### Minggu 6: Basic Reports

**Periode:** 25 Maret - 1 April 2026

#### Tasks

| Task                    | PIC    | Priority | Estimasi |
| ----------------------- | ------ | -------- | -------- |
| Sales report page       | Attila | P0       | 6 jam    |
| Date range picker       | Attila | P0       | 3 jam    |
| Sales chart integration | Attila | P0       | 4 jam    |
| Integration testing     | Yoga   | P0       | 4 jam    |

#### Reports Page Basic

```typescript
// src/pages/ReportsPage.tsx
import { useState } from 'react';
import { useQuery } from '@tanstack/react-query';
import { format, subDays } from 'date-fns';
import { id } from 'date-fns/locale';
import { Calendar, Download } from 'lucide-react';
import { reportsService } from '@/services/reports.service';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { SalesLineChart } from '@/components/features/reports/SalesLineChart';
import { formatCurrency } from '@/utils/formatters';

export function ReportsPage() {
  const [dateRange, setDateRange] = useState({
    start: subDays(new Date(), 7),
    end: new Date(),
  });

  const { data, isLoading } = useQuery({
    queryKey: ['reports', 'sales', dateRange],
    queryFn: () => reportsService.getSalesReport(dateRange.start, dateRange.end),
  });

  return (
    <div className="space-y-6">
      <div className="flex justify-between items-center">
        <h1 className="text-2xl font-bold">Laporan Penjualan</h1>
        <div className="flex gap-4">
          <Button variant="outline">
            <Calendar className="w-4 h-4 mr-2" />
            {format(dateRange.start, 'd MMM', { locale: id })} -{' '}
            {format(dateRange.end, 'd MMM yyyy', { locale: id })}
          </Button>
          <Button variant="outline">
            <Download className="w-4 h-4 mr-2" />
            Export
          </Button>
        </div>
      </div>

      {/* Summary Cards */}
      <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
        <Card>
          <CardHeader className="pb-2">
            <CardTitle className="text-sm font-medium text-gray-500">
              Total Penjualan
            </CardTitle>
          </CardHeader>
          <CardContent>
            <p className="text-2xl font-bold">
              {formatCurrency(data?.total_sales || 0)}
            </p>
          </CardContent>
        </Card>
        <Card>
          <CardHeader className="pb-2">
            <CardTitle className="text-sm font-medium text-gray-500">
              Jumlah Transaksi
            </CardTitle>
          </CardHeader>
          <CardContent>
            <p className="text-2xl font-bold">{data?.total_transactions || 0}</p>
          </CardContent>
        </Card>
        <Card>
          <CardHeader className="pb-2">
            <CardTitle className="text-sm font-medium text-gray-500">
              Rata-rata per Transaksi
            </CardTitle>
          </CardHeader>
          <CardContent>
            <p className="text-2xl font-bold">
              {formatCurrency(data?.average_transaction || 0)}
            </p>
          </CardContent>
        </Card>
        <Card>
          <CardHeader className="pb-2">
            <CardTitle className="text-sm font-medium text-gray-500">
              Items Terjual
            </CardTitle>
          </CardHeader>
          <CardContent>
            <p className="text-2xl font-bold">{data?.total_items || 0}</p>
          </CardContent>
        </Card>
      </div>

      {/* Chart */}
      <Card>
        <CardHeader>
          <CardTitle>Trend Penjualan</CardTitle>
        </CardHeader>
        <CardContent>
          <SalesLineChart data={data?.daily_sales || []} />
        </CardContent>
      </Card>
    </div>
  );
}
```

---

## Fase 2: Intelligence Layer (Minggu 7-12)

### Minggu 7-8: Advanced Reports

**Periode:** 1-15 April 2026

#### Tasks

| Task                        | PIC    | Priority | Estimasi |
| --------------------------- | ------ | -------- | -------- |
| Date range picker component | Attila | P0       | 4 jam    |
| Sales chart with filters    | Attila | P0       | 6 jam    |
| Top products chart          | Attila | P0       | 4 jam    |
| Payment method breakdown    | Attila | P0       | 3 jam    |
| Inventory report page       | Yoga   | P0       | 6 jam    |
| Stock movement history      | Yoga   | P0       | 4 jam    |

#### Charts Implementation

```typescript
// src/components/features/reports/SalesLineChart.tsx
import {
  LineChart,
  Line,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer,
} from 'recharts';
import { format } from 'date-fns';
import { id } from 'date-fns/locale';
import { formatCurrency } from '@/utils/formatters';

interface DailySale {
  date: string;
  total: number;
  transactions: number;
}

export function SalesLineChart({ data }: { data: DailySale[] }) {
  return (
    <ResponsiveContainer width="100%" height={300}>
      <LineChart data={data}>
        <CartesianGrid strokeDasharray="3 3" />
        <XAxis
          dataKey="date"
          tickFormatter={(value) => format(new Date(value), 'd MMM', { locale: id })}
        />
        <YAxis tickFormatter={(value) => `${(value / 1000000).toFixed(1)}jt`} />
        <Tooltip
          formatter={(value: number) => formatCurrency(value)}
          labelFormatter={(label) =>
            format(new Date(label), 'EEEE, d MMMM yyyy', { locale: id })
          }
        />
        <Line
          type="monotone"
          dataKey="total"
          stroke="#3b82f6"
          strokeWidth={2}
          dot={{ r: 4 }}
          name="Penjualan"
        />
      </LineChart>
    </ResponsiveContainer>
  );
}
```

### Minggu 9-10: AI Insight & Alerts

**Periode:** 4-18 April 2026

#### Tasks

| Task                       | PIC    | Priority | Estimasi |
| -------------------------- | ------ | -------- | -------- |
| AI Insight panel component | Attila | P0       | 6 jam    |
| Stock prediction display   | Attila | P0       | 4 jam    |
| Peak hours visualization   | Attila | P0       | 4 jam    |
| Alerts page                | Yoga   | P0       | 6 jam    |
| Alert notifications        | Yoga   | P0       | 4 jam    |

#### AI Insight Panel

```typescript
// src/components/features/dashboard/AIInsightPanel.tsx
import { useQuery } from '@tanstack/react-query';
import { AlertTriangle, TrendingDown, Clock, Sparkles } from 'lucide-react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Badge } from '@/components/ui/badge';
import { insightsService } from '@/services/insights.service';

export function AIInsightPanel() {
  const { data } = useQuery({
    queryKey: ['ai-insights'],
    queryFn: insightsService.getInsights,
    refetchInterval: 60000, // Refresh every minute
  });

  return (
    <Card>
      <CardHeader>
        <CardTitle className="flex items-center gap-2">
          <Sparkles className="w-5 h-5 text-yellow-500" />
          AI Insight
        </CardTitle>
      </CardHeader>
      <CardContent className="space-y-4">
        {/* Low Stock Predictions */}
        {data?.low_stock_predictions?.length > 0 && (
          <div className="space-y-2">
            <h4 className="font-medium flex items-center gap-2 text-red-600">
              <TrendingDown className="w-4 h-4" />
              Prediksi Stok Habis (3 hari)
            </h4>
            <ul className="space-y-1">
              {data.low_stock_predictions.map((item: any) => (
                <li key={item.product_id} className="flex justify-between text-sm">
                  <span>{item.product_name}</span>
                  <Badge variant="destructive">
                    Sisa {item.days_remaining} hari
                  </Badge>
                </li>
              ))}
            </ul>
          </div>
        )}

        {/* Peak Hours */}
        {data?.peak_hours && (
          <div className="space-y-2">
            <h4 className="font-medium flex items-center gap-2 text-blue-600">
              <Clock className="w-4 h-4" />
              Prediksi Jam Sibuk Besok
            </h4>
            <div className="flex gap-2 flex-wrap">
              {data.peak_hours.map((hour: string) => (
                <Badge key={hour} variant="secondary">
                  {hour}
                </Badge>
              ))}
            </div>
          </div>
        )}

        {/* Fraud Alerts */}
        {data?.fraud_alerts?.length > 0 && (
          <div className="space-y-2">
            <h4 className="font-medium flex items-center gap-2 text-orange-600">
              <AlertTriangle className="w-4 h-4" />
              Aktivitas Mencurigakan
            </h4>
            <ul className="space-y-1">
              {data.fraud_alerts.map((alert: any) => (
                <li key={alert.id} className="text-sm text-gray-600">
                  {alert.description}
                </li>
              ))}
            </ul>
          </div>
        )}
      </CardContent>
    </Card>
  );
}
```

### Minggu 11-12: Export & Polish

**Periode:** 18 April - 2 Mei 2026

#### Tasks

| Task                     | PIC    | Priority | Estimasi |
| ------------------------ | ------ | -------- | -------- |
| Export to PDF            | Attila | P0       | 6 jam    |
| Export to Excel          | Attila | P0       | 4 jam    |
| Settings page            | Yoga   | P0       | 4 jam    |
| Responsive design fixes  | Attila | P0       | 6 jam    |
| Performance optimization | Attila | P1       | 4 jam    |
| Bug fixes                | Both   | P0       | 8 jam    |

---

## Fase 3: Security & AI (Minggu 13-16)

### Minggu 13-14: Blockchain & ML Views

**Periode:** 2-16 Mei 2026

#### Tasks

| Task                       | PIC    | Priority | Estimasi |
| -------------------------- | ------ | -------- | -------- |
| Audit trail page           | Attila | P0       | 6 jam    |
| Blockchain verification UI | Attila | P0       | 4 jam    |
| ML prediction display      | Attila | P0       | 4 jam    |
| Stock forecast chart       | Yoga   | P0       | 4 jam    |

### Minggu 15-16: Final Polish

**Periode:** 16-30 Mei 2026

#### Tasks

| Task                     | PIC    | Priority | Estimasi |
| ------------------------ | ------ | -------- | -------- |
| Final bug fixes          | Both   | P0       | 8 jam    |
| Performance optimization | Attila | P0       | 4 jam    |
| Cross-browser testing    | Yoga   | P0       | 4 jam    |
| Documentation            | Both   | P0       | 4 jam    |

---

## Utility Functions

### Formatters

```typescript
// src/utils/formatters.ts
export function formatCurrency(amount: number): string {
    return new Intl.NumberFormat("id-ID", {
        style: "currency",
        currency: "IDR",
        minimumFractionDigits: 0,
        maximumFractionDigits: 0
    }).format(amount);
}

export function formatNumber(num: number): string {
    return new Intl.NumberFormat("id-ID").format(num);
}

export function formatDate(date: string | Date): string {
    return new Intl.DateTimeFormat("id-ID", {
        dateStyle: "medium"
    }).format(new Date(date));
}

export function formatDateTime(date: string | Date): string {
    return new Intl.DateTimeFormat("id-ID", {
        dateStyle: "medium",
        timeStyle: "short"
    }).format(new Date(date));
}

export function formatRelativeTime(date: string | Date): string {
    const rtf = new Intl.RelativeTimeFormat("id", { numeric: "auto" });
    const diff = Date.now() - new Date(date).getTime();
    const minutes = Math.floor(diff / 60000);
    const hours = Math.floor(diff / 3600000);
    const days = Math.floor(diff / 86400000);

    if (minutes < 60) return rtf.format(-minutes, "minute");
    if (hours < 24) return rtf.format(-hours, "hour");
    return rtf.format(-days, "day");
}
```

---

## Code Review Checklist

Sebelum merge PR, pastikan:

- [ ] TypeScript tidak ada error (`npm run type-check`)
- [ ] ESLint tidak ada error (`npm run lint`)
- [ ] Semua tests passing (`npm run test`)
- [ ] UI responsive di mobile dan desktop
- [ ] Loading states ditampilkan
- [ ] Error states di-handle
- [ ] Console tidak ada warning/error

---

## Weekly Collaboration dengan Yoga

### Daily Sync (15 menit)

- Review progress kemarin
- Diskusi blocker
- Assign task hari ini

### Code Review

- Attila review semua PR dari Yoga
- Berikan feedback konstruktif
- Share knowledge dan best practices

### Pair Programming

- Untuk fitur kompleks, pair programming dengan Yoga
- Terutama untuk integrasi API dan state management

---

_Last updated: 18 Februari 2026_
