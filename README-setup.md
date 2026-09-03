# راه‌اندازی — قدم‌به‌قدم

این اپ (`index.html`) داده‌ها را در (Supabase) ذخیره می‌کند و روی (GitHub Pages) زنده منتشر می‌شود.
اگر کلید (Supabase) را وارد نکنید، اپ باز هم کار می‌کند ولی فقط روی همان مرورگر (حالت محلی).

---

## بخش ۱ — ساختن جدول در (Supabase)

۱. وارد داشبورد پروژه شوید و از منوی چپ روی **SQL Editor** بزنید.
۲. **New query** را بزنید، کد زیر را جای‌گذاری کنید و **Run** را بزنید:

```sql
create table if not exists public.app_state (
  id bigint primary key,
  data jsonb,
  updated_at timestamptz default now()
);

alter table public.app_state enable row level security;

create policy "public read"   on public.app_state for select using (true);
create policy "public insert" on public.app_state for insert with check (true);
create policy "public update" on public.app_state for update using (true) with check (true);
```

> این کد یک جدول عمومی می‌سازد که همه می‌توانند بخوانند و بنویسند (بدون لاگین) — دقیقاً چیزی که انتخاب کردید.

---

## بخش ۲ — گرفتن کلید (anon) و گذاشتن در فایل

۳. در (Supabase) از منوی چپ: **Project Settings → API**.
۴. مقدار **Project URL** و **anon public** را کپی کنید.
   - **anon** یک کلید عمومی است و گذاشتنش در کد اشکالی ندارد.
   - **service_role** را هرگز کپی/استفاده نکنید.
۵. فایل `index.html` را باز کنید و این خط را پیدا کنید:

```js
const SUPABASE_ANON_KEY = "PASTE_YOUR_ANON_PUBLIC_KEY_HERE";
```

به‌جای متن داخل گیومه، کلید (anon) خودتان را بگذارید. (آدرس پروژه از قبل گذاشته شده.)

---

## بخش ۳ — انتشار روی (GitHub Pages)

۶. در (github.com) یک ریپوی جدید بسازید (مثلاً `chelle-app`) — می‌تواند (Public) باشد.
۷. دکمهٔ **Add file → Upload files** را بزنید و همین `index.html` را آپلود کنید، بعد **Commit**.
۸. به **Settings → Pages** بروید.
۹. زیر **Source** گزینهٔ **Deploy from a branch** و شاخهٔ `main` و پوشهٔ `/ (root)` را انتخاب و **Save** کنید.
۱۰. یکی‌دو دقیقه صبر کنید؛ لینک سایت شما بالای همان صفحه ظاهر می‌شود:
   `https://USERNAME.github.io/chelle-app/`

باز که کردید، اگر بالای صفحه نوشت **☁ متصل به فضای ابری (Supabase)** یعنی همه‌چیز درست است.

---

## نکات

- نام فایل باید حتماً `index.html` باشد تا (GitHub Pages) آن را نشان دهد.
- چون جدول عمومی است، هرکس لینک و کلید را داشته باشد می‌تواند داده را ببیند/تغییر دهد. برای اپ شخصی مشکلی نیست؛ اگر بعداً خواستید فقط خودتان دسترسی داشته باشید، باید «لاگین» اضافه کنیم (بگویید تا نسخهٔ لاگین‌دار بسازم).
- اگر روزی کلید (anon) لو رفت یا خواستید عوضش کنید، از **Settings → API** در (Supabase) می‌شود کلید را چرخاند (rotate).
