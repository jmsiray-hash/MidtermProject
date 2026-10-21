# MidPro Point of Sale

A CodeIgniter 4 and MySQL point-of-sale application with staff authentication, catalog/customer/staff management, transactional inventory updates, and sales history.

## Requirements

- PHP 8.2 or newer, with `intl`, `mbstring`, `mysqli`, and `fileinfo` enabled.
- MySQL or MariaDB.
- Composer.

## Setup on XAMPP (Windows)

1. Start **Apache** and **MySQL** from the XAMPP Control Panel. This project is configured for XAMPP's local Apache URL; do not run `php spark serve`.
2. Create a MySQL database named `pos_system_midpro` using phpMyAdmin or the MySQL command line.
3. The project `.env` is set to `http://localhost/pos-systemMidPro/public/` and the usual XAMPP MySQL defaults (`root`, blank password). If your MySQL credentials differ, edit `.env`; it is local-only and is ignored by Git.
4. Apply the schema and create the first staff login from PowerShell in the project directory:

   ```powershell
   php spark migrate
   php spark db:seed InitialUserSeeder
   ```

5. Open **http://localhost/pos-systemMidPro/public/** in your browser.
6. Sign in once with the initial account below, then immediately create a personal staff account and change/remove the initial account:

   - Username: `admin`
   - Password: `ChangeMe123!`

   The seeder only creates this account when the `users` table is empty; it never resets an existing password.

## Features

- **Products:** add, list, edit, and delete products; validate and store JPEG/PNG/WEBP images under `public/uploads/products`; manage price and stock.
- **Customers:** create, list, update, and delete customers. Deleting a customer keeps sales history and clears its optional customer reference.
- **Staff:** create, list, edit, and delete staff accounts; hash passwords with PHP's password API; validate/store optional avatars under `public/uploads/avatars`.
- **Authentication:** explicit login/logout routes, session regeneration on login, and an authentication filter for every management route. CSRF protection is enabled globally.
- **Sales:** record sales against a product and optional customer. A database transaction and row lock prevent overselling; stock decrements only when the sale commits.
- **Sales history:** show product, customer (or walk-in), staff member, quantity, amount, and timestamp.
- **Store settings:** update the store name, address, logo, and currency from the Settings page; the selected currency is shown throughout product and sales screens.

The requested schema does not include an archive flag. Products with transaction history cannot be deleted, preserving their referenced sales; products without sales can be deleted.

## Database schema

Run `php spark migrate` to create `products`, `customers`, `users`, `sales`, and the store settings table, including the requested foreign keys. The migrations are in `app/Database/Migrations`.
