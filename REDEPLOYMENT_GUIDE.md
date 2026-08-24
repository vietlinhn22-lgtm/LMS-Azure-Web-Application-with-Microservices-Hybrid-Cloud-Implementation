# LMS Re-deployment Guide - Essential Files Checklist

## 📋 Required Files for Re-deployment on Another PC

### 1. **Main Configuration File** ⭐ CRITICAL
```
📁 /webproject_sem3/.env
```
**Contains:**
- Database connection strings (PostgreSQL, MongoDB)
- Service ports configuration
- JWT secret keys
- API URLs

**Action Required:** ✏️ **MUST UPDATE** these values:
- `POSTGRES_HOST` - Change to new database host
- `MONGODB_HOST` - Change to new database host
- `JWT_SECRET_KEY` - Keep same for consistency or regenerate
- Port numbers if different

---

### 2. **Frontend Environment Files**

#### Frontend Admin:
```
📁 /lms_micro_services/frontend-admin/.env.development
```
**Contains:** `REACT_APP_API_BASE_URL`

#### Frontend Teacher:
```
📁 /lms_micro_services/frontend-teacher/.env.development
```
**Contains:** `REACT_APP_API_BASE_URL`

#### Frontend Student:
```
📁 /lms_micro_services/frontend-student/.env.development
```
**Contains:** `REACT_APP_API_BASE_URL`

**Action Required:** ✏️ **UPDATE** API URLs to point to new server:
```bash
# Example:
REACT_APP_API_BASE_URL=http://NEW_SERVER_IP:PORT/api
```

---

### 3. **Docker Compose Configuration** ⭐ CRITICAL
```
📁 /webproject_sem3/docker-compose.yml
```
**Contains:**
- All service definitions
- Port mappings
- Volume configurations
- Network settings
- Environment variable references

**Action Required:** ✏️ **CHECK** external port mappings if needed

---

### 4. **Nginx Configuration** ⭐ CRITICAL
```
📁 /webproject_sem3/nginx/nginx.conf
```
**Contains:**
- Reverse proxy routing
- Frontend path configurations
- API gateway routing
- Video streaming settings
- CORS settings

**Action Required:** ℹ️ Usually NO CHANGE needed (uses container names)

---

### 5. **Service-Specific Environment Files**

#### Auth Service:
```
📁 /lms_micro_services/auth-service/.env
📁 /lms_micro_services/auth-service/requirements.txt
```

#### Content Service:
```
📁 /lms_micro_services/content-service/.env (if exists)
📁 /lms_micro_services/content-service/requirements.txt
```

#### Assignment Service:
```
📁 /lms_micro_services/assignment-service/.env (if exists)
📁 /lms_micro_services/assignment-service/requirements.txt
```

---

## 🚀 Quick Re-deployment Steps

### Step 1: Clone Repository
```bash
git clone https://github.com/huynq247/lms_mcsrv_runwell.git
cd lms_mcsrv_runwell
```

### Step 2: Update Configuration Files

#### A. Update Main `.env` File:
```bash
nano .env
```
Change:
```bash
# Update database hosts
POSTGRES_HOST=YOUR_NEW_POSTGRES_HOST
MONGODB_HOST=YOUR_NEW_MONGODB_HOST

# Update if different credentials
POSTGRES_PASSWORD=YOUR_PASSWORD
MONGODB_PASSWORD=YOUR_PASSWORD

# Generate new JWT secret (optional)
JWT_SECRET_KEY=your_new_secret_key_here
```

#### B. Update Frontend Environment Files:
```bash
# Admin
nano lms_micro_services/frontend-admin/.env.development
# Change: REACT_APP_API_BASE_URL=http://NEW_IP:PORT/api

# Teacher
nano lms_micro_services/frontend-teacher/.env.development
# Change: REACT_APP_API_BASE_URL=http://NEW_IP:PORT/api

# Student
nano lms_micro_services/frontend-student/.env.development
# Change: REACT_APP_API_BASE_URL=http://NEW_IP:PORT/api
```

### Step 3: Deploy with Docker
```bash
# Build and start all services
docker compose up -d --build

# Check container status
docker ps

# View logs
docker compose logs -f
```

### Step 4: Verify Deployment
```bash
# Check service health
docker ps --format "table {{.Names}}\t{{.Status}}"

# Test API Gateway
curl http://localhost:80/api/health

# Access portals
# Admin:   http://YOUR_IP:PORT/admin/
# Teacher: http://YOUR_IP:PORT/teacher/
# Student: http://YOUR_IP:PORT/student/
```

---

## 📦 Alternative: Use Backup for Quick Deployment

If you want to use the backup you created:

### Option A: Use Docker Image Backup
```bash
# Copy backup file to new server
scp lms_system_backup_20251024_211447.tar.gz user@new_server:/path/

# On new server, restore
cd /path/to/backup
tar -xzf lms_system_backup_20251024_211447.tar.gz
./restore_lms_backup.sh
```

### Option B: Fresh Build from Source
```bash
# Clone repo
git clone https://github.com/huynq247/lms_mcsrv_runwell.git
cd lms_mcsrv_runwell

# Update configs (see Step 2 above)

# Deploy
docker compose up -d --build
```

---

## 📝 Complete File Checklist

### ✅ Configuration Files to Update:
- [ ] `.env` (root directory)
- [ ] `frontend-admin/.env.development`
- [ ] `frontend-teacher/.env.development`
- [ ] `frontend-student/.env.development`
- [ ] `docker-compose.yml` (check port mappings)

### ✅ Files to Copy As-Is:
- [ ] `nginx/nginx.conf`
- [ ] All `requirements.txt` files
- [ ] All `Dockerfile` files
- [ ] All source code files

### ✅ NOT Needed (Will be rebuilt):
- ❌ `node_modules/` folders
- ❌ `__pycache__/` folders
- ❌ `.pyc` files
- ❌ Build artifacts
- ❌ Docker volumes (data will be in databases)

---

## 🔧 Environment Variables Reference

### Main `.env` File Structure:
```bash
# Server Configuration
SERVER_HOST=localhost
SERVER_PORT=8000
API_GATEWAY_PORT=8000

# PostgreSQL Database
POSTGRES_HOST=14.161.50.86        # ← UPDATE THIS
POSTGRES_PORT=25432
POSTGRES_DB=postgres
POSTGRES_USER=admin
POSTGRES_PASSWORD=Mypassword123   # ← UPDATE THIS

# MongoDB Database
MONGODB_HOST=14.161.50.86         # ← UPDATE THIS
MONGODB_PORT=27017
MONGODB_USER=admin
MONGODB_PASSWORD=Mypassword123    # ← UPDATE THIS
MONGODB_DB=content_db
MONGODB_URL=mongodb://admin:Mypassword123@14.161.50.86:27017/content_db?authSource=admin  # ← UPDATE THIS

# Frontend Ports
FRONTEND_ADMIN_PORT=3001
FRONTEND_TEACHER_PORT=3002
FRONTEND_STUDENT_PORT=3003

# Service Ports
AUTH_SERVICE_PORT=8001
CONTENT_SERVICE_PORT=8002
ASSIGNMENT_SERVICE_PORT=8004

# Security
JWT_SECRET_KEY=your_super_secret_jwt_key_change_this_in_production  # ← UPDATE THIS
JWT_ALGORITHM=HS256
```

### Frontend `.env.development` Structure:
```bash
REACT_APP_API_BASE_URL=http://YOUR_SERVER_IP:PORT/api  # ← UPDATE THIS
```

---

## 🎯 Key Points to Remember

1. **Database Hosts are Critical**: Make sure PostgreSQL and MongoDB are accessible from the new server
2. **API URLs in Frontends**: Must point to the correct API Gateway URL
3. **JWT Secret**: Keep the same if migrating with existing users, or all users must re-login
4. **Port Mappings**: Check if ports 80, 3001, 3002, 3003, 8000 are available on new server
5. **Docker Network**: Will be auto-created, no manual configuration needed
6. **Nginx Config**: Uses Docker service names, should work without changes

---

## 🆘 Common Issues During Re-deployment

### Issue 1: Database Connection Failed
**Solution:** Check `POSTGRES_HOST` and `MONGODB_HOST` are correct and accessible

### Issue 2: Frontend Can't Connect to API
**Solution:** Update `REACT_APP_API_BASE_URL` in all frontend `.env.development` files

### Issue 3: Ports Already in Use
**Solution:** Change port mappings in `docker-compose.yml`

### Issue 4: Assignment Service 500 Error
**Solution:** Verify auth-service URL is using service name, not localhost

---

## 📞 Quick Command Reference

```bash
# Stop all services
docker compose down

# Rebuild specific service
docker compose up -d --build SERVICE_NAME

# View logs
docker compose logs -f SERVICE_NAME

# Restart service
docker compose restart SERVICE_NAME

# Check service health
docker ps
docker compose ps

# Access container shell
docker exec -it CONTAINER_NAME bash
```

---

**🎉 You're Ready to Re-deploy!**

Start with Step 1, update the configuration files, and deploy using Docker Compose. Your LMS will be up and running on the new server in minutes!
