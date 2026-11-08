# Custom Card Database (static site + owner admin)

## Files
- `index.html`, `style.css`, `app.js` – public card browser (search, filters, card detail)
- `data/index.json` – list of sets
- `data/sets/<set-id>.json` – one file of cards per set (the "database")
- `images/` – optional uploaded card art (`images/<card-id>.jpg`)
- `admin.html` – owner-only uploader; commits to your repo through the GitHub API

## One-time setup
1. Create a GitHub repo (public), e.g. `rts-database`. Upload all these files to it.
2. Repo **Settings → Pages** → Source: *Deploy from a branch* → `main` / root. Your site appears at `https://<you>.github.io/<repo>/`.
3. Create a token: GitHub **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate**.
   - Repository access: *Only select repositories* → your repo
   - Permissions → Repository → **Contents: Read and write**
   - Set an expiry you're comfortable with.

## Adding cards
Open `https://<you>.github.io/<repo>/admin.html`, enter owner / repo / branch / token, click **Connect**, pick a set (or create a new one), fill the form, then paste a DuelingBook image URL or upload art and **Save**. Each save is a git commit; the live site refreshes in ~1 minute.

## Security notes
- Only someone with your token can write. The page itself is public but useless without it.
- Never paste the token anywhere else. "Remember on this device" stores it in that browser's localStorage; leave it unchecked on shared computers.
- Anyone with write access to the repo can also edit the files in `data/` directly on github.com.

## Limits
- Each set file must stay under ~1 MB (about 3,000 cards) for the admin page to read it, so split huge sets into parts. The number of sets is unlimited.
- The public site loads every set in parallel, which is fine for tens of thousands of cards. If it gets slow, we can switch to loading sets on demand.
- Images: pasting a DuelingBook image URL stores nothing in the repo. Uploads are resized to 600px wide JPEG.
- Card IDs must be unique across all sets.

## Card fields
`id, name, type, subtype, attribute, race, level, atk, def, scale, archetype, effect, image, status, creator, added`
Add more fields by editing `FIELDS` in `admin.html` and the render code in `app.js`.
