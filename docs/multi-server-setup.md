# LSocialCore Multi-Server Setup Guide

## 🏗️ **Architecture Overview**

LSocialCore supports **hybrid deployment** with automatic environment detection:
- **Velocity Proxy**: Central hub with auto-generated keys
- **Paper Servers**: Sub-servers that sync with proxy using shared keys

## 🔐 **Key Authentication System**

Similar to Votifier/Floodgate, LSocialCore uses **HMAC-SHA256** key-based authentication:

```
┌─────────────────┐    auto-generates    ┌──────────────────┐
│   Velocity      │ ────────────────────► │  cross-server.key │
│   Proxy         │                      │  + config files  │
└─────────────────┘                      └──────────────────┘
        │                                         │
        │                                         │ copy key
        ▼                                         ▼
┌─────────────────┐                      ┌──────────────────┐
│ Paper Server 1  │ ◄────────────────────┤ Paper Server 2  │
│ (auto-key: true)│                      │ (auto-key: true) │
└─────────────────┘                      └──────────────────┘
```

## 🚀 **Step-by-Step Setup**

### **1. Velocity Proxy Setup**

1. **Deploy Plugin**: Place `LSocialCore-1.0.0.jar` in `plugins/` folder
2. **Start Velocity**: Plugin auto-detects Velocity environment
3. **Key Generation**: Plugin creates these files automatically:

```
velocity/
├── plugins/
│   ├── LSocialCore/
│   │   ├── cross-server.key          # 🔑 Master key file
│   │   ├── subserver-config-example.yml  # 📋 Config template
│   │   └── config.yml                # ⚙️ Main config
│   └── LSocialCore-1.0.0.jar
```

### **2. Generated Files Example**

**`cross-server.key`**:
```
# LSocialCore Cross-Server Authentication Key
# Generated: 2025-06-20 16:45:00
# Key Type: 256-bit Base64
# Algorithm: HMAC-SHA256

KEY=mF8jK9xR3nP2wQ7vL6hZ8tE5yU4oI1aS9dG0cX2bN7mV6kJ3fH9gT8eR5wP4qL1z
```

**`subserver-config-example.yml`**:
```yaml
# Copy this section to each Paper server's config.yml
cross-server:
  enabled: true
  server-name: "server1"  # 🔄 CHANGE THIS per server
  authentication:
    auto-key: true        # Use key from cross-server.key file
    port: 25580
    proxy-host: "localhost"  # 🔄 Change to proxy IP
  database:
    type: "mysql"         # 🔄 Use MySQL for multi-server
    host: "localhost"
    port: 3306
    database: "lsocial"
    username: "lsocial_user"
    password: "your_password"
```

### **3. Paper Server Configuration**

**Server 1** (`server1/plugins/LSocialCore/config.yml`):
```yaml
# LSocialCore Configuration - Server 1
cross-server:
  enabled: true
  server-name: "server1"      # ✅ Unique per server
  authentication:
    auto-key: true            # Loads from cross-server.key
    port: 25580
    proxy-host: "10.0.0.100"  # Proxy IP address
  
database:
  type: "mysql"               # ✅ Shared MySQL for sync
  host: "10.0.0.200"
  port: 3306
  database: "lsocial_network"
  username: "lsocial_user"
  password: "secure_password_123"
  
friends:
  enabled: true
  max-friends: 50
  cross-server-notifications: true
  
party:
  enabled: true
  max-party-size: 8
  cross-server-parties: true
```

**Server 2** (`server2/plugins/LSocialCore/config.yml`):
```yaml
# LSocialCore Configuration - Server 2
cross-server:
  enabled: true
  server-name: "server2"      # ✅ Different server name
  authentication:
    auto-key: true
    port: 25580
    proxy-host: "10.0.0.100"  # Same proxy IP
  
database:
  type: "mysql"               # ✅ Same database config
  host: "10.0.0.200"
  port: 3306
  database: "lsocial_network"
  username: "lsocial_user"
  password: "secure_password_123"
  
friends:
  enabled: true
  max-friends: 50
  cross-server-notifications: true
  
party:
  enabled: true
  max-party-size: 8
  cross-server-parties: true
```

**Server 3** (`server3/plugins/LSocialCore/config.yml`):
```yaml
# LSocialCore Configuration - Server 3 (Creative)
cross-server:
  enabled: true
  server-name: "creative"     # ✅ Descriptive name
  authentication:
    auto-key: true
    port: 25580
    proxy-host: "10.0.0.100"
  
database:
  type: "mysql"
  host: "10.0.0.200"
  port: 3306
  database: "lsocial_network"
  username: "lsocial_user"
  password: "secure_password_123"
  
friends:
  enabled: true
  max-friends: 100            # ✅ Higher limit for creative
  cross-server-notifications: true
  
party:
  enabled: false              # ✅ Disabled for creative server
```

## 🗄️ **Database Setup**

### **MySQL Configuration** (Recommended for multi-server):

```sql
-- Create database
CREATE DATABASE lsocial_network;

-- Create user
CREATE USER 'lsocial_user'@'%' IDENTIFIED BY 'secure_password_123';
GRANT ALL PRIVILEGES ON lsocial_network.* TO 'lsocial_user'@'%';
FLUSH PRIVILEGES;
```

### **Connection Pool Settings**:
```yaml
database:
  type: "mysql"
  host: "mysql.example.com"
  port: 3306
  database: "lsocial_network"
  username: "lsocial_user"
  password: "secure_password_123"
  pool:
    maximum-pool-size: 10
    minimum-idle: 5
    connection-timeout: 20000
    idle-timeout: 300000
    max-lifetime: 1200000
```

## 🔧 **Manual Key Setup** (Advanced)

If you prefer manual key management:

```yaml
cross-server:
  authentication:
    auto-key: false           # Disable auto-key loading
    secret-key: "mF8jK9xR3nP2wQ7vL6hZ8tE5yU4oI1aS9dG0cX2bN7mV6kJ3fH9gT8eR5wP4qL1z"
```

## 📋 **Velocity Commands**

```bash
# Generate new key (proxy only)
/lsocialcore key generate

# Export key to file
/lsocialcore key export cross-server.key

# Check authentication status
/lsocialcore key info

# Plugin information
/lsocialcore info
```

## 🔍 **Troubleshooting**

### **Authentication Issues**:
```yaml
debug:
  enabled: true
  log-cross-server-events: true
  log-database-queries: true
```

### **Common Problems**:

1. **"Authentication failed"**:
   - Ensure `cross-server.key` is copied to all Paper servers
   - Check proxy IP in Paper server configs
   - Verify port 25580 is not blocked

2. **"Database connection failed"**:
   - Test MySQL connection from each server
   - Check firewall rules
   - Verify user permissions

3. **"Plugin disabled"**:
   - Check console logs for SQLite syntax errors
   - Ensure Java 17+ is used
   - Verify plugin JAR integrity

## 📊 **Network Topology Examples**

### **Small Network** (2-3 servers):
```
Internet → Velocity (25565) → Paper Servers
           │                  ├── Survival (25570)
           │                  ├── Creative (25571)
           │                  └── Minigames (25572)
           │
           └── MySQL Database (3306)
```

### **Large Network** (5+ servers):
```
Internet → Load Balancer → Multiple Velocity Proxies
                         ├── Proxy 1 → Survival Cluster
                         ├── Proxy 2 → Creative Cluster  
                         └── Proxy 3 → Minigames Cluster
                         
Central Database Cluster (MySQL/MariaDB)
```

## ✅ **Verification Steps**

1. **Deploy plugin to Velocity** → Check key generation
2. **Copy key file to Paper servers** → Verify auto-loading
3. **Configure unique server names** → Test recognition
4. **Setup shared MySQL** → Verify cross-server data
5. **Test friend requests** → Confirm cross-server sync
6. **Test party system** → Check cross-server parties

---
