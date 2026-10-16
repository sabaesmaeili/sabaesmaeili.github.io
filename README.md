# tutoringwithsaba.ca

GitHub user site for `sabaesmaeili`, served at `https://tutoringwithsaba.ca/`.

- `index.html` sends visitors to `https://esaba.ca/tutoring/`, keeping any `#session` or `#meet` part, so `tutoringwithsaba.ca/#session` opens the session calendar.
- `404.html` does the same for any unknown path.
- Every other GitHub Pages repo without its own custom domain is served under this domain automatically: the game at `https://tutoringwithsaba.ca/monster-shop/`, future games at `https://tutoringwithsaba.ca/<repo-name>/`. The Website repo keeps `esaba.ca`.

## Setup, step by step

Do the steps in this order. Adding the domain on GitHub before touching DNS stops anyone else from claiming it.

### 1. GitHub: create the site

1. Open https://github.com/new.
2. Repository name: `sabaesmaeili.github.io` (exactly). Public. Create it.
3. In the new repo: **Add file > Upload files**. Drag in `index.html` and `404.html` from this folder. **Commit changes**.
4. **Settings > Pages**: Source **Deploy from a branch**, branch `main`, folder `/ (root)`. **Save**.
5. Same page, **Custom domain**: type `tutoringwithsaba.ca`, **Save**. GitHub adds a `CNAME` file to the repo. A red "DNS check unsuccessful" is expected until step 2 is done.
6. Optional, recommended: verify the domain. Click your profile picture > **Settings > Pages > Add a domain**, enter `tutoringwithsaba.ca`. GitHub shows a TXT record; add it in step 2.4, then click **Verify**.

### 2. Namecheap: point the domain at GitHub

Namecheap > **Domain List** > **Manage** next to `tutoringwithsaba.ca` > **Advanced DNS**.

1. Under **Host Records**, delete the two records Namecheap adds to new domains: `CNAME Record www parkingpage.namecheap.com` and `URL Redirect Record @ http://www.tutoringwithsaba.ca`. These parking records are the usual cause of the headache: they fight GitHub's records.
2. **Add New Record** for each row. The host is `@` for the bare domain; never type the domain name itself. TTL: Automatic.

   | Type | Host | Value |
   | --- | --- | --- |
   | A Record | @ | 185.199.108.153 |
   | A Record | @ | 185.199.109.153 |
   | A Record | @ | 185.199.110.153 |
   | A Record | @ | 185.199.111.153 |
   | CNAME Record | www | sabaesmaeili.github.io. |

3. Click the green check on each row to save it.
4. If you did step 1.6: add a **TXT Record**, host `_github-pages-challenge-sabaesmaeili`, value = the code GitHub showed.

### 3. Wait, then turn on HTTPS

1. DNS usually updates within 30 minutes, occasionally a few hours. Check in Terminal: `dig +short tutoringwithsaba.ca` should print the four `185.199.x.153` addresses.
2. Back in the repo's **Settings > Pages**, wait for "DNS check successful" and for the certificate (GitHub says up to 24 hours, usually much less). Then tick **Enforce HTTPS**.
3. Test:
   - `https://tutoringwithsaba.ca` lands on `https://esaba.ca/tutoring/`
   - `https://www.tutoringwithsaba.ca` forwards to the bare domain
   - `https://tutoringwithsaba.ca/monster-shop/` opens the game

### 4. Email: contact@tutoringwithsaba.ca (iCloud, like esaba.ca)

1. Open https://www.icloud.com/settings/ > **Custom Email Domain** > **Manage** > add `tutoringwithsaba.ca` for **Only you**, then create the address `contact@tutoringwithsaba.ca`.
2. Apple lists DNS records. In Namecheap **Advanced DNS**, scroll to **Mail Settings** and switch the dropdown to **Custom MX**. MX rows can only be added after this switch, the second common headache. Add:

   | Type | Host | Value | Priority |
   | --- | --- | --- | --- |
   | MX Record | @ | mx01.mail.icloud.com. | 10 |
   | MX Record | @ | mx02.mail.icloud.com. | 10 |

   And under **Host Records**:

   | Type | Host | Value |
   | --- | --- | --- |
   | TXT Record | @ | `apple-domain=...` (Apple's exact value) |
   | TXT Record | @ | `v=spf1 include:icloud.com ~all` |
   | CNAME Record | sig1._domainkey | `sig1.dkim.tutoringwithsaba.ca.at.icloudmailadmin.com.` |

   If Apple's screen shows different values, use Apple's. Two TXT records on `@` is fine.
3. Finish the setup in iCloud, then send a test email to `contact@tutoringwithsaba.ca` from another account. The tutoring page's email link points to this address, so check it receives mail before publishing that change.
