<div dir="rtl">

# Hello Solana (Native) (سلام سولانا - بومی)

یک پوشه خالی ایجاد کنید و تمامی وظایف زیر را به ترتیب انجام دهید.

---

## Tasks (وظایف)

- **Build** (ساخت پروژه)
- **Test locally with LiteSVM** (تست محلی با لایت‌اس‌وی‌ام)
- **Deploy locally to solana-test-validator and test with Rust script** (استقرار محلی روی اعتبارسنج آزمایشی سولانا و تست با اسکریپت راست)
- **Deploy to Devnet and run the Rust script again** (استقرار روی شبکه توسعه و اجرای مجدد اسکریپت راست)

---

## Build (ساخت پروژه)

این دستور فایل با پسوند `.so` را در مسیر `target/deploy/` تولید می‌کند:

```bash
cargo build-sbf
```

---

## Test (تست)

برای اجرای تست‌های واحد دستور زیر را اجرا کنید:

```bash
cargo test -- --nocapture
```

---

## Test with script (تست با اسکریپت)

### ۱. اجرای Validator (اعتبارسنج محلی)

ابتدا پیکربندی کلاینت محلی را تنظیم کرده و اعتبارسنج را اجرا کنید:

```bash
solana config set -ul
solana-test-validator
```

### ۲. بررسی Program ID (شناسه برنامه)

جهت دریافت آدرس یا همان **Program ID** (شناسه برنامه) دستور زیر را وارد کنید:

```bash
solana address -k ./target/deploy/hello-keypair.json
```

### ۳. Deploy program (استقرار برنامه)

برنامه کامپایل‌شده را روی اعتبارسنج محلی مستقر کنید:

```bash
solana program deploy ./target/deploy/hello.so
```

### ۴. Execute demo script (اجرای اسکریپت دمو)

متغیرهای محیطی زیر را مقداردهی کرده و اسکریپت دمو را اجرا کنید:

```bash
PROGRAM_ID=your program ID
RPC=http://localhost:8899
KEYPAIR=path to key pair

cargo run --example demo $KEYPAIR $RPC $PROGRAM_ID
```

---

## Deploy to Devnet (استقرار روی شبکه توسعه)

### ۱. تنظیم پیکربندی روی Devnet (شبکه توسعه)

```bash
solana config set -ud
```

### ۲. بررسی موجودی و دریافت Airdrop (ایردراپ)

موجودی کیف پول خود را بررسی کنید و در صورت نیاز درخواست توکن رایگان دهید:

```bash
solana balance
# دریافت ایردراپ در صورت کم بودن موجودی کیف پول
solana airdrop 1
```

### ۳. ساخت مجدد و استقرار

```bash
cargo build-sbf

solana program deploy ./target/deploy/hello.so
```

### ۴. اجرای اسکریپت روی Devnet (شبکه توسعه)

```bash
PROGRAM_ID=your program ID
RPC=https://api.devnet.solana.com
KEYPAIR=path to key pair

cargo run --example demo $KEYPAIR $RPC $PROGRAM_ID
```

### ۵. بررسی Transaction Signature (امضای تراکنش)

**Transaction Signature** (امضای تراکنش) را در مرورگر بلاک‌چین سولانا (**Solana Explorer** (مرورگر سولانا)) جستجو و بررسی کنید.

### ۶. آزادسازی دارایی‌های قفل‌شده

در پایان، برنامه را ببندید تا **SOL** (ارز دیجیتال سولانا) مصرف‌شده برای ذخیره‌سازی را پس بگیرید (**Reclaim** (بازپس‌گیری)):

```bash
solana program close $PROGRAM_ID
```



</div>