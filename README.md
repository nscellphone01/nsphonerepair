# NS Cellphone Store POS

Point of sale for NS Cellphone Store (Edmonton): sales, repairs, phones by IMEI, used-phone buying,
stock, customers and end of day. Live at **https://nsphonerepair.ca**. Phones, tablets and computers
can install it as an app, and it opens with no internet.

```
src/NS_Cellphone_Store.html   the POS as exported from claude.ai (replace this to update the app)
src/ns_cloud_firebase.js      BiixiPay Cloud sync on Firebase (replaces the export's Supabase sync)
scripts/build.py              turns the two into site/index.html
site/                         the website (index.html, offline support sw.js, manifest, icons)
firestore.rules               Firestore security rules (shared Firebase project, see below)
```

## Updating the app

Save the new export from claude.ai over `src/NS_Cellphone_Store.html`, then run:

```bash
python3 scripts/build.py
```

and push to `main`. Cloudflare Pages publishes `site/` automatically, in about a minute.

## Hosting

Cloudflare Pages (project `nsphonerepair`) is connected to this repository: production branch `main`,
no build command, output directory `site`. Custom domains `nsphonerepair.ca` and `www.nsphonerepair.ca`.
Firebase **Authentication → Settings → Authorized domains** must include `nsphonerepair.ca`.

## BiixiPay Cloud (sharing data across devices)

Data is saved in each browser. **Menu → BiixiPay Cloud** syncs it through Firebase (project `mrchef-b8507`,
collection `nsStores`, shared with the Bismillah Restaurant POS). People sign in with email and password; only
members of a store can read or change its data. Firebase needs **Email/Password** sign-in enabled and the rules
in `firestore.rules` published. That file also contains the Bismillah rule, so always publish the whole file.

Owner: **Create account** → **Create & upload this device's data**. Staff: the owner invites their email; on their
device they **Create account** with it, click the confirmation link in the email, then **Join**.

Moving from another address: on the old one use **Menu → Backup → Download backup**, then **Restore from backup** here
(or just sign in to BiixiPay Cloud if that device was syncing).
