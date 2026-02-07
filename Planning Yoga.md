# Planning Yoga - Frontend Developer

## Dokumen Perencanaan Detail untuk Yoga

**Role:** Frontend Developer (Support Attila)  
**Periode:** 18 Februari - 10 Juni 2026 (16 Minggu)

---

## Ringkasan Tanggung Jawab

| Area                      | Deskripsi                                       |
| ------------------------- | ----------------------------------------------- |
| **Component Development** | Membangun reusable UI components                |
| **Page Development**      | Develop halaman-halaman dashboard sesuai assign |
| **Testing**               | Unit testing, integration testing, E2E testing  |
| **Documentation**         | Technical documentation, user guide             |
| **Support**               | Membantu Attila dalam development               |
| **QA**                    | Quality assurance, bug hunting                  |

---

## Learning Path

Karena Yoga akan bekerja dengan React + TypeScript, berikut adalah learning path yang direkomendasikan:

### Minggu 0-1: Fundamentals Review

| Topic             | Resource            | Duration |
| ----------------- | ------------------- | -------- |
| TypeScript Basics | TypeScript Handbook | 4 jam    |
| React Hooks       | React Docs          | 4 jam    |
| React Router      | React Router Docs   | 2 jam    |
| TanStack Query    | TanStack Query Docs | 3 jam    |
| Tailwind CSS      | Tailwind Docs       | 2 jam    |

### Recommended Tutorials

1. **TypeScript dengan React**
    - https://react-typescript-cheatsheet.netlify.app/

2. **TanStack Query**
    - https://tanstack.com/query/latest/docs/react/overview

3. **Tailwind CSS**
    - https://tailwindcss.com/docs

4. **shadcn/ui**
    - https://ui.shadcn.com/docs

---

## Tech Stack yang Digunakan

| Layer       | Teknologi                | Notes                    |
| ----------- | ------------------------ | ------------------------ |
| Framework   | React 18                 | Library utama            |
| Language    | TypeScript               | Untuk type safety        |
| Build Tool  | Vite                     | Fast build tool          |
| Styling     | Tailwind CSS             | Utility-first CSS        |
| Components  | shadcn/ui                | Pre-built components     |
| State       | TanStack Query           | Server state             |
| Testing     | Vitest + Testing Library | Unit & integration tests |
| E2E Testing | Playwright               | End-to-end tests         |

---

## Fase 0: Setup & Learning (Minggu 1)

**Periode:** 18-25 Februari 2026

### Checklist Tasks

#### Day 1-2: Environment Setup

- [ ] Install Node.js 20+ LTS
- [ ] Install VS Code dengan extensions:
    - ESLint
    - Prettier
    - Tailwind CSS IntelliSense
    - TypeScript Importer
    - Error Lens
- [ ] Clone repository dari Van
- [ ] Run `npm install` di folder dashboard
- [ ] Pastikan project bisa running (`npm run dev`)
- [ ] Familiarize dengan folder structure

#### Day 3-4: Learning & Practice

- [ ] Review TypeScript basics
- [ ] Practice React hooks (useState, useEffect, useCallback)
- [ ] Learn TanStack Query basics (useQuery, useMutation)
- [ ] Explore shadcn/ui components
- [ ] Buat simple component sebagai latihan

#### Day 5-6: Documentation Setup

- [ ] Setup documentation folder structure:
    ```
    docs/
    ├── development/
    │   ├── setup-guide.md
    │   ├── coding-standards.md
    │   └── git-workflow.md
    ├── api/
    │   └── endpoints.md
    └── user-guide/
        ├── kasir-guide.md
        └── owner-guide.md
    ```
- [ ] Draft environment setup guide berdasarkan pengalaman setup
- [ ] Document any issues encountered dan solusinya

#### Day 6-7: Component Library Exploration

- [ ] Review shadcn/ui documentation
- [ ] Install semua components yang dibutuhkan
- [ ] Buat sample page menggunakan components
- [ ] Document component usage patterns

### Deliverables Minggu 1

| Deliverable                   | Status |
| ----------------------------- | ------ |
| Environment fully setup       | ☐      |
| Can run project locally       | ☐      |
| TypeScript basics understood  | ☐      |
| React Query basics understood | ☐      |
| Setup guide documented        | ☐      |
| Sample component created      | ☐      |

---

## Fase 1: MVP Foundation (Minggu 2-6)

### Minggu 2-3: Component Library & Support

**Periode:** 25 Februari - 11 Maret 2026

#### Tasks

| Task                            | Priority | Estimasi | Status |
| ------------------------------- | -------- | -------- | ------ |
| Setup testing framework         | P0       | 3 jam    | ☐      |
| Buat LoadingSpinner component   | P0       | 1 jam    | ☐      |
| Buat EmptyState component       | P0       | 2 jam    | ☐      |
| Buat ErrorBoundary component    | P0       | 2 jam    | ☐      |
| Buat PageHeader component       | P0       | 2 jam    | ☐      |
| Buat ConfirmDialog component    | P0       | 2 jam    | ☐      |
| Buat StatusBadge component      | P0       | 1 jam    | ☐      |
| Write tests untuk components    | P0       | 4 jam    | ☐      |
| Support Attila sesuai kebutuhan | P0       | -        | ☐      |

#### Component Implementations

##### LoadingSpinner

```typescript
// src/components/common/LoadingSpinner.tsx
import { Loader2 } from 'lucide-react';
import { cn } from '@/lib/utils';

interface LoadingSpinnerProps {
  size?: 'sm' | 'md' | 'lg';
  className?: string;
}

const sizeClasses = {
  sm: 'w-4 h-4',
  md: 'w-6 h-6',
  lg: 'w-8 h-8',
};

export function LoadingSpinner({ size = 'md', className }: LoadingSpinnerProps) {
  return (
    <Loader2
      className={cn('animate-spin text-primary', sizeClasses[size], className)}
    />
  );
}

// Full page loading
export function PageLoading() {
  return (
    <div className="flex items-center justify-center h-64">
      <LoadingSpinner size="lg" />
    </div>
  );
}
```

##### EmptyState

```typescript
// src/components/common/EmptyState.tsx
import { LucideIcon, Package } from 'lucide-react';
import { Button } from '@/components/ui/button';

interface EmptyStateProps {
  icon?: LucideIcon;
  title: string;
  description?: string;
  actionLabel?: string;
  onAction?: () => void;
}

export function EmptyState({
  icon: Icon = Package,
  title,
  description,
  actionLabel,
  onAction,
}: EmptyStateProps) {
  return (
    <div className="flex flex-col items-center justify-center py-12 text-center">
      <div className="w-16 h-16 rounded-full bg-gray-100 flex items-center justify-center mb-4">
        <Icon className="w-8 h-8 text-gray-400" />
      </div>
      <h3 className="text-lg font-medium text-gray-900 mb-1">{title}</h3>
      {description && (
        <p className="text-sm text-gray-500 max-w-sm mb-4">{description}</p>
      )}
      {actionLabel && onAction && (
        <Button onClick={onAction}>{actionLabel}</Button>
      )}
    </div>
  );
}
```

##### ErrorBoundary

```typescript
// src/components/common/ErrorBoundary.tsx
import React from 'react';
import { AlertTriangle } from 'lucide-react';
import { Button } from '@/components/ui/button';

interface Props {
  children: React.ReactNode;
  fallback?: React.ReactNode;
}

interface State {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends React.Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
    // TODO: Send to error tracking service
  }

  render() {
    if (this.state.hasError) {
      if (this.props.fallback) {
        return this.props.fallback;
      }

      return (
        <div className="flex flex-col items-center justify-center py-12 text-center">
          <div className="w-16 h-16 rounded-full bg-red-100 flex items-center justify-center mb-4">
            <AlertTriangle className="w-8 h-8 text-red-500" />
          </div>
          <h3 className="text-lg font-medium text-gray-900 mb-1">
            Terjadi Kesalahan
          </h3>
          <p className="text-sm text-gray-500 max-w-sm mb-4">
            {this.state.error?.message || 'Something went wrong'}
          </p>
          <Button
            onClick={() => {
              this.setState({ hasError: false });
              window.location.reload();
            }}
          >
            Muat Ulang
          </Button>
        </div>
      );
    }

    return this.props.children;
  }
}
```

##### PageHeader

```typescript
// src/components/common/PageHeader.tsx
import { ReactNode } from 'react';

interface PageHeaderProps {
  title: string;
  description?: string;
  actions?: ReactNode;
}

export function PageHeader({ title, description, actions }: PageHeaderProps) {
  return (
    <div className="flex items-center justify-between mb-6">
      <div>
        <h1 className="text-2xl font-bold text-gray-900">{title}</h1>
        {description && (
          <p className="text-sm text-gray-500 mt-1">{description}</p>
        )}
      </div>
      {actions && <div className="flex items-center gap-3">{actions}</div>}
    </div>
  );
}
```

##### ConfirmDialog

```typescript
// src/components/common/ConfirmDialog.tsx
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
} from '@/components/ui/alert-dialog';

interface ConfirmDialogProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  title: string;
  description: string;
  confirmLabel?: string;
  cancelLabel?: string;
  onConfirm: () => void;
  variant?: 'default' | 'destructive';
  isLoading?: boolean;
}

export function ConfirmDialog({
  open,
  onOpenChange,
  title,
  description,
  confirmLabel = 'Konfirmasi',
  cancelLabel = 'Batal',
  onConfirm,
  variant = 'default',
  isLoading = false,
}: ConfirmDialogProps) {
  return (
    <AlertDialog open={open} onOpenChange={onOpenChange}>
      <AlertDialogContent>
        <AlertDialogHeader>
          <AlertDialogTitle>{title}</AlertDialogTitle>
          <AlertDialogDescription>{description}</AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel disabled={isLoading}>
            {cancelLabel}
          </AlertDialogCancel>
          <AlertDialogAction
            onClick={onConfirm}
            disabled={isLoading}
            className={
              variant === 'destructive'
                ? 'bg-red-500 hover:bg-red-600'
                : undefined
            }
          >
            {isLoading ? 'Memproses...' : confirmLabel}
          </AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  );
}
```

##### StatusBadge

```typescript
// src/components/common/StatusBadge.tsx
import { Badge } from '@/components/ui/badge';
import { cn } from '@/lib/utils';

type StatusType = 'success' | 'warning' | 'error' | 'info' | 'default';

interface StatusBadgeProps {
  status: StatusType;
  label: string;
}

const statusStyles: Record<StatusType, string> = {
  success: 'bg-green-100 text-green-800 hover:bg-green-100',
  warning: 'bg-amber-100 text-amber-800 hover:bg-amber-100',
  error: 'bg-red-100 text-red-800 hover:bg-red-100',
  info: 'bg-blue-100 text-blue-800 hover:bg-blue-100',
  default: 'bg-gray-100 text-gray-800 hover:bg-gray-100',
};

export function StatusBadge({ status, label }: StatusBadgeProps) {
  return (
    <Badge variant="secondary" className={cn(statusStyles[status])}>
      {label}
    </Badge>
  );
}

// Helper untuk transaction status
export function TransactionStatusBadge({
  status,
}: {
  status: 'PENDING' | 'COMPLETED' | 'VOIDED';
}) {
  const config: Record<string, { status: StatusType; label: string }> = {
    PENDING: { status: 'warning', label: 'Pending' },
    COMPLETED: { status: 'success', label: 'Selesai' },
    VOIDED: { status: 'error', label: 'Dibatalkan' },
  };

  const { status: type, label } = config[status] || config.PENDING;
  return <StatusBadge status={type} label={label} />;
}
```

### Minggu 4-5: Employee Management Page

**Periode:** 11-25 Maret 2026

#### Tasks

| Task                     | Priority | Estimasi | Status |
| ------------------------ | -------- | -------- | ------ |
| Employee list page       | P0       | 6 jam    | ☐      |
| Employee table component | P0       | 3 jam    | ☐      |
| Add employee modal       | P0       | 4 jam    | ☐      |
| Edit employee modal      | P0       | 3 jam    | ☐      |
| Delete confirmation      | P0       | 2 jam    | ☐      |
| Role filter              | P1       | 2 jam    | ☐      |
| Unit tests               | P0       | 4 jam    | ☐      |

#### Employee Page Implementation

```typescript
// src/pages/EmployeesPage.tsx
import { useState } from 'react';
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { Plus, Search, Edit, Trash2, MoreVertical } from 'lucide-react';
import { usersService } from '@/services/users.service';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu';
import { DataTable } from '@/components/common/DataTable';
import { PageHeader } from '@/components/common/PageHeader';
import { StatusBadge } from '@/components/common/StatusBadge';
import { ConfirmDialog } from '@/components/common/ConfirmDialog';
import { EmployeeModal } from '@/components/features/employees/EmployeeModal';
import type { User } from '@/types/user.types';

export function EmployeesPage() {
  const queryClient = useQueryClient();
  const [search, setSearch] = useState('');
  const [roleFilter, setRoleFilter] = useState<string>('all');
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [selectedEmployee, setSelectedEmployee] = useState<User | null>(null);
  const [deleteTarget, setDeleteTarget] = useState<User | null>(null);

  // Fetch employees
  const { data, isLoading } = useQuery({
    queryKey: ['employees', search, roleFilter],
    queryFn: () =>
      usersService.getAll({
        search,
        role: roleFilter !== 'all' ? roleFilter : undefined,
      }),
  });

  // Delete mutation
  const deleteMutation = useMutation({
    mutationFn: usersService.delete,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['employees'] });
      setDeleteTarget(null);
    },
  });

  const columns = [
    {
      header: 'Nama',
      accessor: 'full_name',
      cell: (value: string, row: User) => (
        <div>
          <p className="font-medium">{value}</p>
          <p className="text-sm text-gray-500">@{row.username}</p>
        </div>
      ),
    },
    {
      header: 'Role',
      accessor: 'role',
      cell: (value: string) => {
        const roleColors: Record<string, 'info' | 'warning' | 'success'> = {
          KASIR: 'info',
          ADMIN: 'warning',
          OWNER: 'success',
        };
        return <StatusBadge status={roleColors[value] || 'default'} label={value} />;
      },
    },
    {
      header: 'Status',
      accessor: 'is_active',
      cell: (value: boolean) => (
        <StatusBadge
          status={value ? 'success' : 'default'}
          label={value ? 'Aktif' : 'Nonaktif'}
        />
      ),
    },
    {
      header: 'Aksi',
      accessor: 'id',
      cell: (_: number, row: User) => (
        <DropdownMenu>
          <DropdownMenuTrigger asChild>
            <Button variant="ghost" size="sm">
              <MoreVertical className="w-4 h-4" />
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            <DropdownMenuItem
              onClick={() => {
                setSelectedEmployee(row);
                setIsModalOpen(true);
              }}
            >
              <Edit className="w-4 h-4 mr-2" />
              Edit
            </DropdownMenuItem>
            <DropdownMenuItem
              className="text-red-600"
              onClick={() => setDeleteTarget(row)}
            >
              <Trash2 className="w-4 h-4 mr-2" />
              Hapus
            </DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
      ),
    },
  ];

  return (
    <div className="space-y-6">
      <PageHeader
        title="Karyawan"
        description="Kelola akun karyawan dan hak akses"
        actions={
          <Button
            onClick={() => {
              setSelectedEmployee(null);
              setIsModalOpen(true);
            }}
          >
            <Plus className="w-4 h-4 mr-2" />
            Tambah Karyawan
          </Button>
        }
      />

      {/* Filters */}
      <div className="flex gap-4">
        <div className="relative flex-1 max-w-sm">
          <Search className="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" />
          <Input
            placeholder="Cari karyawan..."
            value={search}
            onChange={(e) => setSearch(e.target.value)}
            className="pl-10"
          />
        </div>
        <Select value={roleFilter} onValueChange={setRoleFilter}>
          <SelectTrigger className="w-40">
            <SelectValue placeholder="Semua Role" />
          </SelectTrigger>
          <SelectContent>
            <SelectItem value="all">Semua Role</SelectItem>
            <SelectItem value="KASIR">Kasir</SelectItem>
            <SelectItem value="ADMIN">Admin</SelectItem>
            <SelectItem value="OWNER">Owner</SelectItem>
          </SelectContent>
        </Select>
      </div>

      {/* Table */}
      <DataTable columns={columns} data={data?.data || []} isLoading={isLoading} />

      {/* Add/Edit Modal */}
      <EmployeeModal
        open={isModalOpen}
        onOpenChange={setIsModalOpen}
        employee={selectedEmployee}
      />

      {/* Delete Confirmation */}
      <ConfirmDialog
        open={!!deleteTarget}
        onOpenChange={(open) => !open && setDeleteTarget(null)}
        title="Hapus Karyawan"
        description={`Apakah Anda yakin ingin menghapus ${deleteTarget?.full_name}? Aksi ini tidak dapat dibatalkan.`}
        confirmLabel="Hapus"
        variant="destructive"
        isLoading={deleteMutation.isPending}
        onConfirm={() => deleteTarget && deleteMutation.mutate(deleteTarget.id)}
      />
    </div>
  );
}
```

#### Employee Modal Implementation

```typescript
// src/components/features/employees/EmployeeModal.tsx
import { useEffect } from 'react';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { useMutation, useQueryClient } from '@tanstack/react-query';
import { usersService } from '@/services/users.service';
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';
import type { User } from '@/types/user.types';

const employeeSchema = z.object({
  username: z
    .string()
    .min(3, 'Username minimal 3 karakter')
    .regex(/^[a-z0-9_]+$/, 'Username hanya boleh huruf kecil, angka, dan underscore'),
  full_name: z.string().min(2, 'Nama minimal 2 karakter'),
  password: z.string().min(6, 'Password minimal 6 karakter').optional(),
  role: z.enum(['KASIR', 'ADMIN', 'OWNER']),
  is_active: z.boolean(),
});

type EmployeeForm = z.infer<typeof employeeSchema>;

interface EmployeeModalProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  employee: User | null;
}

export function EmployeeModal({
  open,
  onOpenChange,
  employee,
}: EmployeeModalProps) {
  const queryClient = useQueryClient();
  const isEditing = !!employee;

  const {
    register,
    handleSubmit,
    reset,
    setValue,
    watch,
    formState: { errors },
  } = useForm<EmployeeForm>({
    resolver: zodResolver(
      isEditing
        ? employeeSchema.omit({ password: true }).extend({
            password: z.string().min(6).optional().or(z.literal('')),
          })
        : employeeSchema
    ),
    defaultValues: {
      username: '',
      full_name: '',
      password: '',
      role: 'KASIR',
      is_active: true,
    },
  });

  // Populate form when editing
  useEffect(() => {
    if (employee) {
      reset({
        username: employee.username,
        full_name: employee.full_name,
        role: employee.role,
        is_active: employee.is_active,
        password: '',
      });
    } else {
      reset({
        username: '',
        full_name: '',
        password: '',
        role: 'KASIR',
        is_active: true,
      });
    }
  }, [employee, reset]);

  const createMutation = useMutation({
    mutationFn: usersService.create,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['employees'] });
      onOpenChange(false);
    },
  });

  const updateMutation = useMutation({
    mutationFn: ({ id, data }: { id: number; data: Partial<EmployeeForm> }) =>
      usersService.update(id, data),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['employees'] });
      onOpenChange(false);
    },
  });

  const onSubmit = (data: EmployeeForm) => {
    if (isEditing) {
      const updateData: Partial<EmployeeForm> = {
        full_name: data.full_name,
        role: data.role,
        is_active: data.is_active,
      };
      if (data.password) {
        updateData.password = data.password;
      }
      updateMutation.mutate({ id: employee.id, data: updateData });
    } else {
      createMutation.mutate(data as Required<EmployeeForm>);
    }
  };

  const isLoading = createMutation.isPending || updateMutation.isPending;

  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>
            {isEditing ? 'Edit Karyawan' : 'Tambah Karyawan'}
          </DialogTitle>
        </DialogHeader>

        <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
          <div className="space-y-2">
            <Label htmlFor="username">Username</Label>
            <Input
              id="username"
              {...register('username')}
              disabled={isEditing}
              placeholder="contoh: kasir01"
            />
            {errors.username && (
              <p className="text-red-500 text-sm">{errors.username.message}</p>
            )}
          </div>

          <div className="space-y-2">
            <Label htmlFor="full_name">Nama Lengkap</Label>
            <Input
              id="full_name"
              {...register('full_name')}
              placeholder="Nama lengkap karyawan"
            />
            {errors.full_name && (
              <p className="text-red-500 text-sm">{errors.full_name.message}</p>
            )}
          </div>

          <div className="space-y-2">
            <Label htmlFor="password">
              Password {isEditing && '(kosongkan jika tidak diubah)'}
            </Label>
            <Input
              id="password"
              type="password"
              {...register('password')}
              placeholder={isEditing ? '••••••••' : 'Minimal 6 karakter'}
            />
            {errors.password && (
              <p className="text-red-500 text-sm">{errors.password.message}</p>
            )}
          </div>

          <div className="space-y-2">
            <Label>Role</Label>
            <Select
              value={watch('role')}
              onValueChange={(value) =>
                setValue('role', value as 'KASIR' | 'ADMIN' | 'OWNER')
              }
            >
              <SelectTrigger>
                <SelectValue />
              </SelectTrigger>
              <SelectContent>
                <SelectItem value="KASIR">Kasir</SelectItem>
                <SelectItem value="ADMIN">Admin</SelectItem>
                <SelectItem value="OWNER">Owner</SelectItem>
              </SelectContent>
            </Select>
          </div>

          <div className="flex justify-end gap-3 pt-4">
            <Button
              type="button"
              variant="outline"
              onClick={() => onOpenChange(false)}
              disabled={isLoading}
            >
              Batal
            </Button>
            <Button type="submit" disabled={isLoading}>
              {isLoading ? 'Menyimpan...' : 'Simpan'}
            </Button>
          </div>
        </form>
      </DialogContent>
    </Dialog>
  );
}
```

### Minggu 6: Integration Testing

**Periode:** 25 Maret - 1 April 2026

#### Tasks

| Task                        | Priority | Estimasi | Status |
| --------------------------- | -------- | -------- | ------ |
| Setup Vitest                | P0       | 2 jam    | ☐      |
| Setup Testing Library       | P0       | 2 jam    | ☐      |
| Unit tests untuk components | P0       | 4 jam    | ☐      |
| Integration tests           | P0       | 4 jam    | ☐      |
| Bug fixing dari tests       | P0       | 4 jam    | ☐      |

#### Testing Setup

```typescript
// vitest.config.ts
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";
import path from "path";

export default defineConfig({
    plugins: [react()],
    test: {
        environment: "jsdom",
        globals: true,
        setupFiles: ["./src/test/setup.ts"],
        coverage: {
            provider: "v8",
            reporter: ["text", "json", "html"]
        }
    },
    resolve: {
        alias: {
            "@": path.resolve(__dirname, "./src")
        }
    }
});
```

```typescript
// src/test/setup.ts
import "@testing-library/jest-dom";
import { vi } from "vitest";

// Mock window.matchMedia
Object.defineProperty(window, "matchMedia", {
    writable: true,
    value: vi.fn().mockImplementation((query) => ({
        matches: false,
        media: query,
        onchange: null,
        addListener: vi.fn(),
        removeListener: vi.fn(),
        addEventListener: vi.fn(),
        removeEventListener: vi.fn(),
        dispatchEvent: vi.fn()
    }))
});
```

#### Sample Tests

```typescript
// src/components/common/__tests__/StatusBadge.test.tsx
import { render, screen } from '@testing-library/react';
import { StatusBadge, TransactionStatusBadge } from '../StatusBadge';

describe('StatusBadge', () => {
  it('renders with correct label', () => {
    render(<StatusBadge status="success" label="Aktif" />);
    expect(screen.getByText('Aktif')).toBeInTheDocument();
  });

  it('applies success styles', () => {
    render(<StatusBadge status="success" label="Success" />);
    const badge = screen.getByText('Success');
    expect(badge).toHaveClass('bg-green-100');
  });

  it('applies error styles', () => {
    render(<StatusBadge status="error" label="Error" />);
    const badge = screen.getByText('Error');
    expect(badge).toHaveClass('bg-red-100');
  });
});

describe('TransactionStatusBadge', () => {
  it('shows correct label for COMPLETED', () => {
    render(<TransactionStatusBadge status="COMPLETED" />);
    expect(screen.getByText('Selesai')).toBeInTheDocument();
  });

  it('shows correct label for VOIDED', () => {
    render(<TransactionStatusBadge status="VOIDED" />);
    expect(screen.getByText('Dibatalkan')).toBeInTheDocument();
  });
});
```

```typescript
// src/components/common/__tests__/ConfirmDialog.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { ConfirmDialog } from '../ConfirmDialog';
import { vi } from 'vitest';

describe('ConfirmDialog', () => {
  const defaultProps = {
    open: true,
    onOpenChange: vi.fn(),
    title: 'Confirm Action',
    description: 'Are you sure?',
    onConfirm: vi.fn(),
  };

  it('renders title and description', () => {
    render(<ConfirmDialog {...defaultProps} />);
    expect(screen.getByText('Confirm Action')).toBeInTheDocument();
    expect(screen.getByText('Are you sure?')).toBeInTheDocument();
  });

  it('calls onConfirm when confirm button clicked', () => {
    render(<ConfirmDialog {...defaultProps} />);
    fireEvent.click(screen.getByText('Konfirmasi'));
    expect(defaultProps.onConfirm).toHaveBeenCalled();
  });

  it('shows loading state', () => {
    render(<ConfirmDialog {...defaultProps} isLoading />);
    expect(screen.getByText('Memproses...')).toBeInTheDocument();
  });

  it('disables buttons when loading', () => {
    render(<ConfirmDialog {...defaultProps} isLoading />);
    expect(screen.getByText('Memproses...')).toBeDisabled();
  });
});
```

---

## Fase 2: Intelligence Layer (Minggu 7-12)

### Minggu 7-8: Inventory Report

**Periode:** 1-15 April 2026

#### Tasks

| Task                      | Priority | Estimasi | Status |
| ------------------------- | -------- | -------- | ------ |
| Inventory report page     | P0       | 6 jam    | ☐      |
| Stock level visualization | P0       | 4 jam    | ☐      |
| Stock movement table      | P0       | 4 jam    | ☐      |
| Low stock filter          | P1       | 2 jam    | ☐      |

### Minggu 9-10: Alerts & Notifications

**Periode:** 4-18 April 2026

#### Tasks

| Task                       | Priority | Estimasi | Status |
| -------------------------- | -------- | -------- | ------ |
| Alerts page                | P0       | 6 jam    | ☐      |
| Alert list component       | P0       | 3 jam    | ☐      |
| Alert detail modal         | P0       | 3 jam    | ☐      |
| Notification dropdown      | P0       | 4 jam    | ☐      |
| Mark as read functionality | P1       | 2 jam    | ☐      |

#### Alerts Page Implementation

```typescript
// src/pages/AlertsPage.tsx
import { useState } from 'react';
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { Bell, Check, AlertTriangle, TrendingDown, Clock } from 'lucide-react';
import { alertsService } from '@/services/alerts.service';
import { Button } from '@/components/ui/button';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
import { PageHeader } from '@/components/common/PageHeader';
import { AlertCard } from '@/components/features/alerts/AlertCard';
import { EmptyState } from '@/components/common/EmptyState';
import type { Alert } from '@/types/alert.types';

export function AlertsPage() {
  const queryClient = useQueryClient();
  const [tab, setTab] = useState<'all' | 'unread'>('unread');

  const { data, isLoading } = useQuery({
    queryKey: ['alerts', tab],
    queryFn: () => alertsService.getAll({ unread_only: tab === 'unread' }),
  });

  const markAllReadMutation = useMutation({
    mutationFn: alertsService.markAllAsRead,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['alerts'] });
    },
  });

  const alerts = data?.data || [];
  const unreadCount = alerts.filter((a: Alert) => !a.is_read).length;

  return (
    <div className="space-y-6">
      <PageHeader
        title="Alerts"
        description="Notifikasi dan peringatan dari sistem"
        actions={
          unreadCount > 0 && (
            <Button
              variant="outline"
              onClick={() => markAllReadMutation.mutate()}
              disabled={markAllReadMutation.isPending}
            >
              <Check className="w-4 h-4 mr-2" />
              Tandai Semua Dibaca
            </Button>
          )
        }
      />

      <Tabs value={tab} onValueChange={(v) => setTab(v as 'all' | 'unread')}>
        <TabsList>
          <TabsTrigger value="unread">
            Belum Dibaca {unreadCount > 0 && `(${unreadCount})`}
          </TabsTrigger>
          <TabsTrigger value="all">Semua</TabsTrigger>
        </TabsList>

        <TabsContent value={tab} className="mt-4">
          {isLoading ? (
            <div className="space-y-3">
              {[...Array(3)].map((_, i) => (
                <div key={i} className="h-20 bg-gray-100 rounded animate-pulse" />
              ))}
            </div>
          ) : alerts.length === 0 ? (
            <EmptyState
              icon={Bell}
              title="Tidak ada alerts"
              description={
                tab === 'unread'
                  ? 'Semua alerts sudah dibaca'
                  : 'Belum ada alerts dari sistem'
              }
            />
          ) : (
            <div className="space-y-3">
              {alerts.map((alert: Alert) => (
                <AlertCard key={alert.id} alert={alert} />
              ))}
            </div>
          )}
        </TabsContent>
      </Tabs>
    </div>
  );
}
```

### Minggu 11-12: Export & Settings

**Periode:** 18 April - 2 Mei 2026

#### Tasks

| Task                     | Priority | Estimasi | Status |
| ------------------------ | -------- | -------- | ------ |
| Export to Excel function | P0       | 4 jam    | ☐      |
| Export to PDF function   | P0       | 4 jam    | ☐      |
| Settings page            | P0       | 4 jam    | ☐      |
| Profile settings         | P1       | 3 jam    | ☐      |
| E2E testing setup        | P0       | 4 jam    | ☐      |
| E2E test scenarios       | P0       | 4 jam    | ☐      |

#### Export Utilities

```typescript
// src/utils/export.ts
import * as XLSX from "xlsx";

export function exportToExcel<T extends Record<string, any>>(
    data: T[],
    filename: string,
    sheetName: string = "Sheet1"
) {
    const worksheet = XLSX.utils.json_to_sheet(data);
    const workbook = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(workbook, worksheet, sheetName);

    // Generate buffer
    const excelBuffer = XLSX.write(workbook, { bookType: "xlsx", type: "array" });

    // Create blob and download
    const blob = new Blob([excelBuffer], {
        type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
    });
    const url = URL.createObjectURL(blob);
    const link = document.createElement("a");
    link.href = url;
    link.download = `${filename}.xlsx`;
    link.click();
    URL.revokeObjectURL(url);
}

// For PDF, we'll use the browser's print functionality
export function exportToPDF(elementId: string, filename: string) {
    const element = document.getElementById(elementId);
    if (!element) return;

    const printWindow = window.open("", "_blank");
    if (!printWindow) return;

    printWindow.document.write(`
    <!DOCTYPE html>
    <html>
      <head>
        <title>${filename}</title>
        <style>
          body { font-family: Arial, sans-serif; }
          table { width: 100%; border-collapse: collapse; }
          th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
          th { background-color: #f3f4f6; }
        </style>
      </head>
      <body>
        ${element.innerHTML}
      </body>
    </html>
  `);

    printWindow.document.close();
    printWindow.print();
}
```

---

## Fase 3: Security & AI (Minggu 13-16)

### Minggu 13-14: Support ML Views

**Periode:** 2-16 Mei 2026

#### Tasks

| Task                            | Priority | Estimasi | Status |
| ------------------------------- | -------- | -------- | ------ |
| Stock forecast chart            | P0       | 4 jam    | ☐      |
| Prediction confidence indicator | P1       | 2 jam    | ☐      |
| Help Attila dengan audit trail  | P0       | 4 jam    | ☐      |
| Testing blockchain verification | P0       | 4 jam    | ☐      |

### Minggu 15-16: Documentation & QA

**Periode:** 16-30 Mei 2026

#### Tasks

| Task                      | Priority | Estimasi | Status |
| ------------------------- | -------- | -------- | ------ |
| Cross-browser testing     | P0       | 4 jam    | ☐      |
| Mobile responsive testing | P0       | 4 jam    | ☐      |
| User guide - Kasir        | P0       | 4 jam    | ☐      |
| User guide - Owner        | P0       | 4 jam    | ☐      |
| Technical documentation   | P0       | 4 jam    | ☐      |
| Final bug fixes           | P0       | 8 jam    | ☐      |

#### User Guide Template

```markdown
# Panduan Pengguna AIPOS - Kasir

## Daftar Isi

1. Login ke Aplikasi
2. Membuka Shift
3. Melakukan Transaksi
4. Pembayaran
5. Menutup Shift
6. Troubleshooting

---

## 1. Login ke Aplikasi

### Langkah-langkah:

1. Buka aplikasi AIPOS di perangkat Anda
2. Masukkan username yang diberikan admin
3. Masukkan password
4. Tap tombol "Masuk"

### Catatan:

- Jika lupa password, hubungi admin
- Aplikasi akan otomatis logout setelah 30 menit tidak aktif

---

## 2. Membuka Shift

### Langkah-langkah:

1. Setelah login, Anda akan masuk ke halaman "Buka Shift"
2. Masukkan jumlah uang modal awal di kasir
3. Tap tombol "Mulai Shift"

### Tips:

- Pastikan menghitung uang fisik dengan benar
- Modal awal ini akan dicatat sebagai pembanding saat tutup shift

---

## 3. Melakukan Transaksi

### Menambah Produk ke Keranjang:

**Cara 1: Scan Barcode**

1. Tap icon barcode di kolom pencarian
2. Arahkan kamera ke barcode produk
3. Produk otomatis masuk ke keranjang

**Cara 2: Cari Manual**

1. Ketik nama produk di kolom pencarian
2. Pilih produk dari daftar
3. Tap tombol "+" untuk menambah

### Mengubah Jumlah:

- Tap "-" atau "+" di keranjang
- Atau tap angka untuk input manual

### Menghapus Item:

- Geser item ke kiri dan tap "Hapus"

---

## 4. Pembayaran

### Pembayaran Tunai:

1. Tap tombol "Bayar"
2. Pilih "Tunai"
3. Masukkan jumlah uang dari pelanggan
4. Sistem akan menghitung kembalian
5. Tap "Konfirmasi"
6. Cetak atau kirim struk

### Pembayaran QRIS:

1. Tap tombol "Bayar"
2. Pilih "QRIS"
3. Tampilkan QR code ke pelanggan
4. Tunggu konfirmasi pembayaran
5. Cetak atau kirim struk

---

## 5. Menutup Shift

### Langkah-langkah:

1. Tap menu (≡) di pojok kanan atas
2. Pilih "Tutup Shift"
3. Masukkan jumlah uang fisik di kasir
4. Review ringkasan shift
5. Tap "Konfirmasi Tutup Shift"

### Yang Perlu Diperhatikan:

- Selisih kas akan tercatat
- Pastikan semua transaksi sudah selesai

---

## 6. Troubleshooting

### Aplikasi Tidak Bisa Login

- Periksa koneksi internet
- Pastikan username dan password benar
- Coba restart aplikasi

### Transaksi Gagal

- Jika offline, transaksi tetap tersimpan
- Akan otomatis sync saat online
- Lihat status sync di pojok kanan atas

### Struk Tidak Tercetak

- Pastikan printer terhubung via Bluetooth
- Coba matikan dan nyalakan printer
- Hubungi admin jika masalah berlanjut
```

---

## Testing Checklist

### Unit Tests

| Component      | Test Cases                    | Status |
| -------------- | ----------------------------- | ------ |
| LoadingSpinner | Render, sizes                 | ☐      |
| EmptyState     | Render, with action           | ☐      |
| PageHeader     | Render, with actions          | ☐      |
| StatusBadge    | All variants                  | ☐      |
| ConfirmDialog  | Open, close, confirm, loading | ☐      |
| DataTable      | Empty, loading, with data     | ☐      |

### Integration Tests

| Flow           | Test Cases                   | Status |
| -------------- | ---------------------------- | ------ |
| Login          | Success, error, validation   | ☐      |
| Employee CRUD  | Create, read, update, delete | ☐      |
| Product search | Search, filter, pagination   | ☐      |
| Alert actions  | Mark read, view detail       | ☐      |

### E2E Tests (Playwright)

| Scenario     | Steps                                   | Status |
| ------------ | --------------------------------------- | ------ |
| Login flow   | Enter credentials → Submit → Dashboard  | ☐      |
| Add employee | Open form → Fill → Submit → See in list | ☐      |
| View report  | Select date → View chart → Export       | ☐      |

---

## Weekly Checklist

Setiap akhir minggu, Yoga harus:

- [ ] Semua assigned tasks selesai
- [ ] Unit tests untuk components baru
- [ ] Kode sudah di-review oleh Attila
- [ ] Tidak ada console errors/warnings
- [ ] PR sudah merge ke develop
- [ ] Update documentation jika perlu
- [ ] Report progress ke Attila

---

## Communication Guidelines

### Kapan Tanya Attila

- Stuck lebih dari 30 menit
- Perlu keputusan desain/arsitektur
- Menemukan bug yang bukan area kamu

### Kapan Tanya Van

- Masalah terkait API
- Error yang tidak bisa dijelaskan
- Butuh data tambahan dari backend

### Kapan Tanya Wulan

- Kurang jelas dengan desain
- Perlu asset/icon
- Ingin validasi implementasi UI

---

_Last updated: 18 Februari 2026_
