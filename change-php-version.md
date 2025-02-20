If you installed **MAMP** (for PHP 5 + Apache) and **Herd** (for PHP 8 + Nginx), they **can coexist** on your Mac, but you need to **configure them properly** to avoid conflicts.

---

## **Key Considerations:**
1. **MAMP and Herd Use Different Servers:**
   - **MAMP** uses **Apache** and provides PHP 5.
   - **Herd** uses **Nginx** and provides PHP 8.

2. **Port Conflicts Between Apache and Nginx:**
   - **MAMP (Apache)** usually runs on **port 80 (or 8888 by default in MAMP settings)**.
   - **Herd (Nginx)** may also try to use **port 80**.
   - You need to **set them to use different ports** to avoid conflicts.

3. **PHP CLI (Command Line) Version Conflict:**
   - By default, `php -v` may point to either **MAMP’s PHP 5** or **Herd’s PHP 8**.
   - You need to manually switch versions when using the command line.

---

# **How to Configure MAMP and Herd to Work Together**

### **Step 1: Change Apache’s Port in MAMP**
To prevent conflict with Nginx:
1. Open **MAMP**.
2. Go to **Preferences → Ports**.
3. Change Apache’s port to **8888** (or any unused port).
4. Click **Save** and restart MAMP.

Now, MAMP (Apache) will be accessible at:
```
http://localhost:8888
```

---

### **Step 2: Change Nginx’s Port in Herd (If Needed)**
By default, Herd's Nginx may also try to use port 80. To avoid conflicts:
1. Open Herd’s config file:
   ```bash
   nano ~/.herd/config.json
   ```
2. Look for:
   ```json
   "nginx_port": 80
   ```
   **Change it to an unused port** (e.g., **8080**):
   ```json
   "nginx_port": 8080
   ```
3. Save the file (**Ctrl + X**, then **Y**, then **Enter**).
4. Restart Herd:
   ```bash
   herd restart
   ```
Now, Herd’s Nginx will run at:
```
http://localhost:8080
```

---

### **Step 3: Set PHP CLI Version Based on Needs**
Since you have **both PHP 5 (MAMP) and PHP 8 (Herd)**, you need to switch versions manually when using the command line.

#### **(A) Use MAMP's PHP 5 in Terminal**
To always use **PHP 5 from MAMP**:
```bash
echo 'export PATH="/Applications/MAMP/bin/php/php5.6.40/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```
Verify:
```bash
php -v
```
It should show **PHP 5.6**.

#### **(B) Use Herd's PHP 8 in Terminal**
To switch to **PHP 8 from Herd**:
```bash
echo 'export PATH="/Users/toe/Library/Application Support/Herd/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```
Verify:
```bash
php -v
```
It should show **PHP 8.4**.

**To switch between PHP versions**, just edit `~/.zshrc` or use `export PATH` in your terminal.

---

### **Step 4: Restart Both Servers**
1. **Restart MAMP (Apache)**
   ```bash
   sudo apachectl restart
   ```
   Or restart it from the MAMP app.

2. **Restart Herd (Nginx)**
   ```bash
   herd restart
   ```

Now, both should be working **without conflicts**:
- **MAMP (PHP 5 + Apache)** → `http://localhost:8888`
- **Herd (PHP 8 + Nginx)** → `http://localhost:8080`

---

## **Final Check**
- Run:
  ```bash
  php -v
  ```
  - If you see **PHP 5**, it’s using MAMP.
  - If you see **PHP 8**, it’s using Herd.

- Open your browser and check:
  - `http://localhost:8888` (MAMP - Apache + PHP 5)
  - `http://localhost:8080` (Herd - Nginx + PHP 8)

Everything should work without conflicts. 🚀

Let me know if you need more help! 😊
