# Green Basket — setup guide (mobile-friendly)

This project is a working React storefront with a Supabase backend. It includes editable products, prices, inventory, store settings, store-wide sale controls, bank-transfer/COD checkout, order storage and admin order management.

## Important: demo mode vs live mode
- Before Supabase is configured, the site shows sample products only. It does not save real orders or publish changes.
- After completing steps below and deploying, products, settings and orders are stored in Supabase.
- This starter accepts a bank-transfer reference but does NOT verify transfers automatically. Check your account before confirming an order.
- It does not charge cards or integrate an online payment gateway.

## Step 1 — Create the free backend
1. Open https://supabase.com/ and create a project.
2. Save the database password somewhere safe. Do not share it.
3. In the project dashboard, open **SQL Editor** → **New query**.
4. Open `supabase/schema.sql` from this project, copy all its contents and paste them into the SQL editor.
5. Press **Run**. Confirm the tables `products`, `store_settings`, and `orders` exist under Table Editor.

## Step 2 — Create your private admin login
1. In Supabase, open **Authentication** → **Users**.
2. Choose **Add user** / **Create new user**.
3. Enter your own email and a strong password. Confirm/create the user if prompted.
4. Keep this account private. Do not enable public sign-ups for this simple starter.
5. In the dashboard, open Authentication settings and ensure public sign-ups are disabled if the setting is available. If you later need customer accounts, implement a proper role system first.

**Security note:** the SQL starter uses authenticated-user policies for admin operations. Only create trusted users. For a public commercial store, use a dedicated admin-role table/policies and server-side order validation before accepting valuable orders.

## Step 3 — Configure the store details
1. In Supabase **Project Settings** → **API** (sometimes labelled **Data API**), copy the Project URL and the publishable/anon key.
2. Copy `.env.example` to a new file named `.env` at the project root.
3. Put your URL in `VITE_SUPABASE_URL` and your publishable/anon key in `VITE_SUPABASE_ANON_KEY`.
4. Optionally put your WhatsApp number, digits only including country code, in `VITE_STORE_WHATSAPP`.
5. Never put the Supabase `service_role` / secret key in a frontend `.env` file or public repository.
6. In Supabase Table Editor → `store_settings`, update the bank name, account title and IBAN/account number with details you intend customers to see. You can also update these from the Store admin after logging in.

## Step 4 — Run locally
Requires a computer with Node.js 20 or newer:
```bash
npm install
npm run dev
```
Open the local address shown in the terminal. To check a production build:
```bash
npm run build
npm run preview
```
The app is a standard Vite React project. Android-only users may find GitHub Codespaces or another cloud development environment easier than installing Node on a phone.

## Step 5 — Deploy at low cost
1. Upload this project to a private or public GitHub repository (do not commit `.env`).
2. Create a Netlify account and choose **Add new site** → **Import an existing project** → connect the repository.
3. Build command: `npm run build`. Publish directory: `dist`.
4. In Netlify site configuration → Environment variables, add:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
   - `VITE_STORE_WHATSAPP` (optional)
5. Trigger a new deploy after setting variables.
6. Open the deployed site and test a sample order with a test address before advertising it.

Free plan limits, product terms and availability can change. A custom domain generally costs extra.

## How to manage products, prices and sales
1. Open your live store and click **Admin**.
2. Sign in using the private Supabase admin user from Step 2.
3. **Products → Add product** to add an item.
4. Click the settings icon next to a product to edit its name, category, price, old price, image URL, description, stock or visibility.
5. Click the trash icon to delete a product.
6. **Store & sale** lets you change store name, announcement, contact details, delivery fee and bank details.
7. To run a sale, tick **Activate store-wide sale**, enter a discount percentage (0–90), and choose **Save & publish settings**. The sale is reflected in the public storefront. Untick it and save to end the sale.
8. For a product-specific sale, fill in **Old price** while editing a product. Use a genuine previous price; don't mislead customers.
9. **Orders** lists orders and lets you change status. For bank transfer, verify the payment independently before marking confirmed/processing.

## Before real customers use it
- Replace sample products and photos with items you actually sell or have permission to use.
- Add your real contact details, shipping policy, return/refund policy and privacy notice.
- Test the site on a phone and test the complete order flow.
- The current frontend submits totals/items from the browser. For a high-volume/production store, move price/stock validation to a trusted server or Supabase Edge Function, rate-limit checkout, add spam protection and validate order payloads server-side. Otherwise a technically savvy customer could tamper with prices.
- Set up daily backups and review Supabase/Netlify plan limits.
- Payment confirmation is manual; this is not a payment gateway integration.

## File map
- `src/main.jsx` — storefront, cart, checkout and admin interface
- `src/style.css` — responsive visual styling
- `supabase/schema.sql` — database tables, row-level security policies and sample products
- `.env.example` — required environment variables
