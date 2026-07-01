# CertChain clean run notes

## Normal local run

```bash
cp .env.example .env
composer install
npm install
php artisan key:generate
php artisan storage:link
php artisan migrate --seed
npm run dev
php artisan serve
```

Open: `http://127.0.0.1:8000`

## Required PHP extensions

Laravel needs a normal PHP web stack. Make sure these extensions are enabled on the target machine/hosting:

- `mbstring`
- `openssl`
- `pdo_mysql`
- `fileinfo`
- `tokenizer`
- `ctype`
- `json`
- `xml`, `dom`, `xmlwriter` for developer tools/tests such as Pint/PHPUnit

On Ubuntu this is typically:

```bash
sudo apt install php-mbstring php-mysql php-xml php-fileinfo
```

## What was cleaned/fixed

- Removed dependency on the old custom startup scripts from the deliverable.
- Removed real `.env` secrets from the deliverable. Use `.env.example` and generate a fresh `APP_KEY`.
- Fixed dashboard search queries that referenced non-existing columns.
- Fixed notification queries that referenced non-existing timestamps/fields.
- Fixed dashboard and public verification so certificate ID and proof fingerprint/hash can be checked.
- Fixed public `/verify` route so homepage verify links no longer hit a missing route.
- Fixed certificate issuance to use `config/blockchain.php` instead of a missing `services.ethereum` config key.
- Replaced direct QR file writes with Laravel storage disk writes.
- Removed hardcoded sample student values from the add-certificate form.
- Restricted the Users page/controller to admin users only.
- Changed logout route to POST only.
- Escaped notification text before rendering it into HTML to reduce XSS risk.
- Replaced broad `$guarded = []` usage in key models with explicit `$fillable` fields.

## Blockchain status

The current code records a local proof row and a local transaction reference. It does not contain a working Node/Hardhat/Sepolia submission service. To enable real Sepolia commits, add a dedicated blockchain service/job that signs and sends the transaction, then save the real transaction hash/block number to `blockchain_records`.
