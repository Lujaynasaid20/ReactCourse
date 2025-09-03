Absolutely, lujayna! You're building a modern React project with a powerful stack—TypeScript, Tailwind, ShadCN UI, React Hook Form, TanStack Query & Table, React Router, and Axios. Here's a step-by-step guide to get you rolling with a clean, scalable frontend that interacts with a user API.

---

## 🛠️ 1. Project Setup

```bash
# Create React + TypeScript app
npx create-react-app user-dashboard --template typescript
cd user-dashboard

# Install dependencies
npm install tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Install core libraries
npm install axios react-router-dom@6 react-hook-form @tanstack/react-query @tanstack/react-table

# Install ShadCN UI
npm install -D shadcn-ui
npx shadcn-ui@latest init
```

---

## 🎨 2. Configure TailwindCSS

Update `tailwind.config.js`:

```js
module.exports = {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
    "./node_modules/shadcn-ui/**/*.{js,ts,jsx,tsx}"
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

In `src/index.css`, replace everything with:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## 🧭 3. Set Up Routing

Create a basic routing structure in `src/App.tsx`:

```tsx
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import UserForm from './pages/UserForm';

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/add-user" element={<UserForm />} />
      </Routes>
    </Router>
  );
}

export default App;
```

---

## 📡 4. Axios Setup

Create `src/api/axios.ts`:

```ts
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://your-api-url.com/api',
});

export default api;
```

---

## 🔄 5. React Query Setup

In `src/main.tsx`:

```tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById('root')!).render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
);
```

---

## 👥 6. User Form with React Hook Form

Create `src/pages/UserForm.tsx`:

```tsx
import { useForm } from 'react-hook-form';
import api from '../api/axios';

type User = {
  name: string;
  email: string;
  age: number;
  address: string;
  university: string;
  course: string;
};

export default function UserForm() {
  const { register, handleSubmit } = useForm<User>();

  const onSubmit = async (data: User) => {
    await api.post('/users', data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4 p-4">
      <input {...register('name')} placeholder="Name" className="input" />
      <input {...register('email')} placeholder="Email" className="input" />
      <input {...register('age')} type="number" placeholder="Age" className="input" />
      <input {...register('address')} placeholder="Address" className="input" />
      <input {...register('university')} placeholder="University" className="input" />
      <input {...register('course')} placeholder="Course" className="input" />
      <button type="submit" className="btn">Submit</button>
    </form>
  );
}
```

---

## 📊 7. Display Users with TanStack Table

Create `src/pages/Home.tsx`:

```tsx
import { useQuery } from '@tanstack/react-query';
import { useReactTable, getCoreRowModel } from '@tanstack/react-table';
import api from '../api/axios';

type User = {
  name: string;
  email: string;
  age: number;
  address: string;
  university: string;
  course: string;
};

export default function Home() {
  const { data = [] } = useQuery<User[]>(['users'], async () => {
    const res = await api.get('/users');
    return res.data;
  });

  const columns = [
    { accessorKey: 'name', header: 'Name' },
    { accessorKey: 'email', header: 'Email' },
    { accessorKey: 'age', header: 'Age' },
    { accessorKey: 'address', header: 'Address' },
    { accessorKey: 'university', header: 'University' },
    { accessorKey: 'course', header: 'Course' },
  ];

  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
  });

  return (
    <table className="table-auto w-full">
      <thead>
        {table.getHeaderGroups().map(headerGroup => (
          <tr key={headerGroup.id}>
            {headerGroup.headers.map(header => (
              <th key={header.id}>{header.column.columnDef.header}</th>
            ))}
          </tr>
        ))}
      </thead>
      <tbody>
        {table.getRowModel().rows.map(row => (
          <tr key={row.id}>
            {row.getVisibleCells().map(cell => (
              <td key={cell.id}>{cell.getValue()}</td>
            ))}
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

---

## 🧪 8. Polish with ShadCN Components

Use ShadCN UI components like `Input`, `Button`, `Card`, etc., to replace raw HTML elements. For example:

```tsx
import { Input } from 'shadcn-ui';
<Input {...register('name')} placeholder="Name" />
```

---

## 🚀 9. Final Touches

- Add loading and error states with React Query.
- Use Tailwind for responsive design.
- Add navigation links using `<Link>` from `react-router-dom`.
- Validate form inputs with `react-hook-form`.

---

Want me to help you scaffold this into a GitHub-ready structure or add authentication next?
