# Phase 3.2 – Active Directory & DNS Configuration

## Date

2026-06-16

## Status

🟡 Ready

---

## Objective

Connect to `bp-winserver` via RDP, promote it to a Domain Controller for `blackphoenix.lab`, and configure the foundational Active Directory and DNS infrastructure.

---

## Step 1 – RDP Connection (Mac)

Since you are using a Mac, you will need an RDP client to connect to the Windows Server GUI.

1. Open the **App Store** on your Mac.
2. Search for and download **Microsoft Remote Desktop**.
3. Open the application and click the **+** (Add) button -> **Add PC**.
4. In the **PC name** field, enter the Public IP: `172.200.164.250`.
5. Click **Add**.
6. Double-click the newly created PC to connect.
7. Enter the credentials:
   - Username: `azureadmin`
   - Password: `<The secure password you created>`
8. Accept the certificate warning and connect.

---

## Step 2 – Rename Server (BP-DC01)

1. Once logged in, Server Manager should open automatically.
2. Click **Local Server** on the left menu.
3. Click the auto-generated computer name next to **Computer name**.
4. Click **Change...**
5. Enter `BP-DC01` as the new Computer name.
6. Click **OK**, then **Close**, then **Restart Now**.
7. Wait 2-3 minutes, then reconnect via RDP.

---

## Step 3 – Install AD DS and DNS Roles

1. Open **Server Manager**.
2. Click **Add roles and features**.
3. Click **Next** until you reach the **Server Roles** screen.
4. Check the box for **Active Directory Domain Services**.
5. Click **Add Features** when prompted.
6. Check the box for **DNS Server**.
7. Click **Add Features** when prompted.
8. Click **Next** through the remaining screens and click **Install**.
9. Wait for the installation to finish.

---

## Step 4 – Promote to Domain Controller

1. In Server Manager, click the **Flag icon** with a yellow warning triangle at the top.
2. Click **Promote this server to a domain controller**.
3. Select **Add a new forest**.
4. Root domain name: `blackphoenix.lab`
5. Click **Next**.
6. Enter a Directory Services Restore Mode (DSRM) password (use your main password for the lab).
7. Click **Next** through the DNS delegation warning.
8. Accept the default NetBIOS name (`BLACKPHOENIX`).
9. Click **Next** through the paths and review screens.
10. Click **Install**.
11. The server will restart automatically. This restart takes longer than usual.

---

## Step 5 – Post-Install Verification

1. Reconnect via RDP using your domain credentials:
   - Username: `BLACKPHOENIX\azureadmin`
   - Password: `<Your password>`
2. Open **Server Manager** -> **Tools** -> **Active Directory Users and Computers**.
3. Verify the `blackphoenix.lab` domain is present and functional.

---

## Exit Criteria

- [ ] Connected via RDP successfully.
- [ ] Server renamed to `BP-DC01`.
- [ ] AD DS and DNS roles installed.
- [ ] Server promoted to Domain Controller for `blackphoenix.lab`.
- [ ] Successfully logged in with Domain Admin credentials.
