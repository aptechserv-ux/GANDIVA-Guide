# GANDIVA Guide — Credential Store

> **Andhra Pradesh Police · Technical Services · NATGRID**  
> This repository stores SHA-256 credential hashes for the Gandiva Guide application.  
> **No plaintext passwords are stored here.**

---

## How Authentication Works

1. Officer opens `Gandiva_Guide_AP_Police.html` in a browser
2. Login screen fetches `credentials.json` from this repository
3. The entered password is hashed (SHA-256) locally in the browser
4. Hash is compared against the stored hash — access granted if matched and account is active
5. Session lasts 4 hours, then auto-expires

---

## Managing Credentials

### Revoke a user (immediate effect)
Find their entry in `credentials.json` and change `"a":true` to `"a":false`:
```json
{"u":"APNG0042","h":"7364fd51...","a":false}
```
Commit and push. Takes effect on next login attempt by that user.

### Reactivate a user
Change `"a":false` back to `"a":true`. Commit and push.

### Add a new user
1. Generate SHA-256 of `USERNAME:PASSWORD` (e.g. using https://emn178.github.io/online-tools/sha256.html)
2. Add entry to `credentials.json`:
```json
{"u":"APNG1001","h":"<sha256-hash-here>","a":true}
```
3. Commit and push.

### Bulk credential reset
Re-run the credential generation script (admin only) and replace `credentials.json`.

---

## File Structure

```
GANDIVA-Guide/
├── credentials.json        ← SHA-256 hashes (safe to be public)
└── README.md               ← This file
```

The HTML application file is distributed separately to officers and is NOT stored here.

---

## Security Notes

- Only SHA-256 hashes are stored — the actual passwords cannot be recovered from hashes
- GitHub's full commit history provides an audit trail of all credential changes
- Each change shows: who changed it, what was changed, and when
- Officers are locked out for **30 minutes** after 5 consecutive failed attempts
- Sessions expire after **4 hours** of login

---

## Credential Format Reference

| Field | Value |
|---|---|
| `u` | Username (e.g. `APNG0001`) |
| `h` | SHA-256 of `USERNAME:PASSWORD` |
| `a` | `true` = Active, `false` = Revoked |

---

*Maintained by AP Police Technical Services*
