#Final Project
Project Setup & Run
1. Open the project folder
cd CertChainPro
2. Start the database service
sudo systemctl start mariadb 2>/dev/null || sudo systemctl start mysql
3. Open MySQL
sudo mysql
4. Create database and user
CREATE DATABASE IF NOT EXISTS certchain CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE USER IF NOT EXISTS 'certchain_user'@'localhost' IDENTIFIED BY 'CertChain@12345';
CREATE USER IF NOT EXISTS 'certchain_user'@'127.0.0.1' IDENTIFIED BY 'CertChain@12345';

ALTER USER 'certchain_user'@'localhost' IDENTIFIED BY 'CertChain@12345';
ALTER USER 'certchain_user'@'127.0.0.1' IDENTIFIED BY 'CertChain@12345';

GRANT ALL PRIVILEGES ON certchain.* TO 'certchain_user'@'localhost';
GRANT ALL PRIVILEGES ON certchain.* TO 'certchain_user'@'127.0.0.1';

FLUSH PRIVILEGES;

EXIT;
5. Install dependencies
composer install
npm install
6. Generate application key
php artisan key:generate --force
7. Clear cache and optimize
php artisan optimize:clear
8. Run database migrations and seeders
php artisan migrate --seed
9. Create storage link
php artisan storage:link || true
10. Build frontend assets
npm run build
11. Run the project
php artisan serve --host=127.0.0.1 --port=8000
Open the project in your browser:

http://127.0.0.1:8000
Demo Login
Email: test@example.com
Password: password
