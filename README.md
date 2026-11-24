# Project Organogram: setup guide

This folder is a complete website. It runs on two free services:

- **Supabase** stores the data, the user accounts and the change history.
- **GitHub Pages** hosts the page itself, so it gets a normal web address.

Files in this folder:

- `index.html`: the website
- `setup.sql`: creates the database (run once)
- `import-existing-data.sql`: copies in the projects and people you already entered in the Claude version (optional)
- `README.md`: this guide

Plan on about 30 minutes for the first setup.

---

## Part 1: Create the database (Supabase)

1. Go to supabase.com and sign up for a free account.
2. Create a new project.
   - Pick any name and a strong database password (save it somewhere).
   - Choose the region closest to your team, for example Singapore or Mumbai.
   - Wait a minute or two until the project is ready.
3. Open **SQL Editor** in the left menu and start a new query.
4. Open `setup.sql` in a text editor (Notepad is fine).
   - Near the top, replace `you@example.com` with your own email address. This makes you the admin.
   - Copy the whole file, paste it into the SQL Editor and press **Run**. You should see "Success".
5. Optional: to bring over your existing data, start another new query, paste the whole of `import-existing-data.sql` and press **Run**.

## Part 2: Lock down sign-ups and create your account

Only people you create accounts for can sign in. This avoids needing an email service.

1. Go to **Authentication** and open the sign-in or providers settings.
2. Turn off **Allow new users to sign up** and save.
3. Go to **Authentication > Users**, choose **Add user > Create new user**.
   - Use the same email you put in `setup.sql` and choose a password.
   - Tick **Auto Confirm User**, then create it.

Repeat step 3 later for each colleague who needs to sign in. Send them their password, and they can change it in the app with **Change password**.

## Part 3: Connect the website to the database

1. In Supabase, open **Project Settings** and find the **API** or **API Keys** section.
2. Copy two values:
   - the **Project URL**, which looks like `https://abcdefgh.supabase.co`
   - the **publishable** key (it starts with `sb_publishable_`). If you only see a key called **anon public**, use that.
3. Open `index.html` in a text editor and search for `PASTE_`.
4. Replace the two placeholders with your values, keeping the quotes:

   ```js
   var SUPABASE_URL="https://abcdefgh.supabase.co";
   var SUPABASE_KEY="sb_publishable_...";
   ```

5. Save the file.

These two values are meant to be public, so it is fine that anyone can see them in the page. The database rules in `setup.sql` decide what each person can see and change. Never put the **secret** or **service_role** key in the page.

## Part 4: Put the website online (GitHub Pages)

1. Sign up for a free account at github.com.
2. Create a new repository, for example `organogram`.
   - Set it to **Public**, because free accounts can only publish public repositories. The page holds no data; the data stays in Supabase behind sign-in.
3. In the repository, choose **Add file > Upload files**.
   - Upload `index.html` only.
   - Keep the `.sql` files private, on your computer.
   - Commit the upload.
4. Go to **Settings > Pages**.
   - Under **Build and deployment**, choose **Deploy from a branch**.
   - Pick the `main` branch and the `/ (root)` folder, then save.
5. After a minute, the page shows your web address, for example `https://yourname.github.io/organogram/`.

Open it, sign in, and you are live.

To update the site later, upload a new `index.html` to the same repository.

---

## Using it

- **Projects and people:** as the admin you can create projects and people, and assign anyone.
- **Sharing a project:** open the project and press **Share**. From there you can:
  - **Copy the link** to that project. Each project has its own address ending in `?project=...`.
  - **Turn on view-by-link**, so anyone with the link can see that project without signing in. They can't change anything.
  - **Add people by email** as **Can view** or **Can edit**. They also need an account (Part 2, step 3).
    - Viewers see only the projects shared with them.
    - Editors can change the projects they have edit access to, and add people.
- **History:** press **History** on a project to see every change, when it happened and who made it.
  - **Go back to here** puts the project back exactly as it was right after that change.
  - Going back is recorded as well, so you can undo it the same way.
  - Card moves are hidden by default; tick **Show card moves** to see them.
- **Allocation:** a person's total always counts all their core projects. Someone who can't see a project still sees the correct total, without being told which project the rest is on.

## Things to know about the free plan

- Supabase pauses a free project after a week with no activity. If that happens, sign in to supabase.com, open the project and press **Resume**. Your data is kept.
- The free plan allows two active projects per account. This tool only uses one.
- There is no automatic backup on the free plan. Now and then, in Supabase go to **Table Editor**, open each table and export it to CSV.
