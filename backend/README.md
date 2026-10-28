# Backend

The backend is an Express API using Prisma with MySQL. It can run locally with
Node.js or as Vercel Functions. Product images are uploaded to Vercel Blob; the
API does not rely on a local uploads directory.

## Local development

1. Install dependencies from this directory:

   ```sh
   npm install
   ```

2. Copy `.env.example` to `.env` and set `DATABASE_URL`, `JWT_SECRET`, and any
   other credentials needed by the features you use. `DATABASE_URL` must be a
   complete MySQL connection URL; dotenv does not substitute `${DB_HOST}` or
   similar placeholders inside it. Percent-encode reserved characters in the
   username or password.
3. Apply the committed Prisma migrations to the selected MySQL database:

   ```sh
   npx prisma migrate deploy
   ```

4. Start the API:

   ```sh
   npm run dev
   ```

The API listens on port `5000` by default. The frontend connects to it directly
unless `VITE_API_URL=http://localhost:5173` is set to route requests through
the Vite development proxy.

## Vercel deployment

Deploy the frontend and API as separate Vercel projects from this repository:

1. Create a Vercel project with the repository's `backend` directory as its
   Root Directory. Its API routes are served by `api/index.js`.
2. Add a MySQL database through a Vercel Marketplace provider and set
   `DATABASE_URL` in the backend project's environment variables. Vercel
   hosting does not include a built-in MySQL database. Use an account with an
   authentication plugin supported by Prisma, such as MySQL's
   `caching_sha2_password`. Prisma does not support `sha256_password`; if the
   account uses it, database queries (including products and site settings)
   fail until the database account or provider is corrected. Do not compose
   `DATABASE_URL` from `${DB_*}` placeholders; provide the complete URL.
   To check the account plugin from an administrative MySQL session:

   ```sql
   SELECT user, host, plugin
   FROM mysql.user
   WHERE user = 'YOUR_DATABASE_USER';
   ```

   If it reports `sha256_password`, update that account (replace the host with
   the matching host from the query and use the database password):

   ```sql
   ALTER USER 'YOUR_DATABASE_USER'@'YOUR_DATABASE_HOST'
     IDENTIFIED WITH caching_sha2_password BY 'YOUR_DATABASE_PASSWORD';
   ```

   If the provider does not allow changing authentication plugins, create a
   database account with a Prisma-supported plugin or use a compatible MySQL
   provider. Update the complete `DATABASE_URL` after changing the account
   password.
3. Set `JWT_SECRET` and `FRONTEND_URL` in the backend project. `FRONTEND_URL`
   should be the deployed frontend origin; set payment and mail credentials
   too if those features are enabled.
4. Create a Vercel Blob store for the backend project and ensure
   `BLOB_READ_WRITE_TOKEN` is available to it. Product image upload and delete
   operations use Blob URLs directly.
5. Create a Vercel project with the `frontend` directory as its Root Directory.
   Set `VITE_API_URL` to the backend project's deployment origin, then
   redeploy the frontend because Vite embeds this value at build time.
6. Apply Prisma migrations to the production database before serving traffic:

   ```sh
   cd backend
   npx prisma migrate deploy
   ```

No Railway service or local filesystem image storage is used by this setup.
