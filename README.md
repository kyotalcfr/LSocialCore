# 🚀 LSocialCore - Advanced Friends & Party System

## ✨ Features

### 🌐 Cross-Server Network
- **Multi-Server Friends System** - Players stay connected across your entire network
- **Real-Time Synchronization** - Instant status updates between servers
- **Velocity + Paper Integration** - Seamless proxy-server communication
- **Advanced Session Tracking** - Smart player detection across servers

### 👥 Advanced Friends System
- **Interactive GUI** with server-aware friend status
- **Smart Friend Suggestions** from network players
- **Cross-Server Notifications** - Know when friends join different servers
- **Confirmation System** - Prevent accidental friend removal
- **Cooldown Protection** - Anti-spam friend request system

### 🎉 Party System
- **Private Parties** with password protection and custom names
- **Cross-Server Party Management** - Invite players from any server
- **Party Teleportation** with confirmation dialogs
- **Leader Controls** - Kick, invite, and manage party members
- **Smart Party GUI** with member status and server info

### 🔧 Enterprise Features
- **Database Flexibility** - MySQL & SQLite support with HikariCP pooling
- **Addon System** - Extend functionality with custom addons
- **Debug Commands** - Real-time network monitoring and troubleshooting
- **Resource Leak Protection** - Professional-grade memory management
- **Developer API** - Integrate with other plugins seamlessly

## 🏗️ Installation

### Requirements
- **Server**: Paper 1.21+ or Velocity Proxy 3.3+
- **Java**: 17 or higher
- **Database**: MySQL 8.0+ or SQLite 3.40+
- **Memory**: 512MB+ recommended for large networks

### Quick Setup

1. **Download** the latest release from Polymart/Builtbybit
2. **Upload** `LSocialCore-1.0.0-dist.jar` to your plugins folder
3. **Restart** your server
4. **Configure** settings in `plugins/LSocialCore/config.yml`
5. **Enjoy** professional social features!

### Multi-Server Setup

For cross-server functionality:

1. **Install** LSocialCore on all Paper servers
2. **Install** LSocialCore on your Velocity proxy
3. **Configure** shared database in all configs
4. **Generate** cross-server authentication key
5. **Restart** all servers

## ⚙️ Configuration

### Basic Configuration
```yaml
# Database Configuration
database:
  type: "mysql"  # or "sqlite"
  host: "localhost"
  port: 3306
  database: "lsocialcore"
  username: "root"
  password: "password"

# Friends System
friends:
  enabled: true
  max-friends: 50
  request-timeout: 300
  cross-server: true
  
# Party System  
party:
  enabled: true
  max-party-size: 8
  party-teleport: true
  cross-server: true

# Cross-Server Settings
cross-server:
  enabled: true
  server-name: "lobby"
  authentication:
    enabled: true
    auto-key: true
```

### Advanced Configuration
```yaml
# Friends GUI Settings
friends:
  gui:
    enabled: true
    title: "&6&lFriends Menu"
    size: 54
    update-interval: 20
  suggestions:
    enabled: true
    max-suggestions: 45
    cache-duration: 30

# Party Privacy Settings
party:
  private:
    enabled: true
    allow-password-protection: true
    max-password-length: 16
    allow-named-parties: true
    max-party-name-length: 32

# Debug & Monitoring
debug:
  enabled: false
  log-database-queries: false
  log-cross-server-events: false
```

## 📝 Commands

### Player Commands

#### Friends System
```
/friends                    # Open friends GUI
/friends add <player>       # Send friend request
/friends accept <player>    # Accept friend request
/friends deny <player>      # Deny friend request  
/friends remove <player>    # Remove friend
/friends list              # List all friends
/friends toggle            # Toggle notifications
```

#### Party System
```
/party                           # Open party GUI
/party create                    # Create party
/party private <password> [name] # Create private party
/party invite <player>           # Invite player
/party join <player>             # Join party by invite
/party join <name> <password>    # Join private party
/party leave                     # Leave party
/party kick <player>             # Kick member (leader only)
/party disband                   # Disband party (leader only)
/party list                      # List party members
/party tp <player>               # Teleport to member
/party tphere                    # Summon all members (leader only)
```

### Admin Commands

#### Paper Server Commands
```
/lsocialcore info                     # Plugin information
/lsocialcore reload                   # Reload configuration
/lsocialcore network status          # Network status
/lsocialcore network refresh         # Force refresh network data
/lsocialcore network players         # List network players
/lsocialcore network sessions        # Show database sessions
```

#### Velocity Proxy Commands  
```
/lsocialcore info                     # Network information
/lsocialcore reload                   # Reload key manager
/lsocialcore key generate             # Generate new authentication key
/lsocialcore key export <file>        # Export key file
/lsocialcore key info                 # Key status
```

## 📚 Tutorial

### Setting Up Your First Multi-Server Network

#### Step 1: Database Setup
```sql
-- Create database
CREATE DATABASE lsocialcore;
CREATE USER 'lsocial'@'%' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON lsocialcore.* TO 'lsocial'@'%';
FLUSH PRIVILEGES;
```

#### Step 2: Velocity Configuration
1. Install `LSocialCore-Velocity.jar` in Velocity plugins folder
2. Start Velocity to generate config
3. Configure `plugins/lsocialcore/config.yml`:
```yaml
cross-server:
  enabled: true
  authentication:
    enabled: true
    auto-key: true
    port: 25580
```

#### Step 3: Paper Server Configuration
1. Install `LSocialCore-1.0.0-shaded.jar` on all Paper servers
2. Configure each server with same database:
```yaml
database:
  type: "mysql"
  host: "your-database-host"
  database: "lsocialcore"
  username: "lsocial"
  password: "your_password"

cross-server:
  enabled: true
  server-name: "lobby"  # Change per server: "survival", "creative", etc.
```

#### Step 4: Testing
1. Start Velocity proxy
2. Start all Paper servers
3. Check logs for "Cross-server authentication successful"
4. Test friend requests between servers
5. Verify party functionality across servers

### Common Issues & Solutions

#### Database Connection Issues
```yaml
# For MySQL connection issues, try:
database:
  properties:
    useSSL: false
    allowPublicKeyRetrieval: true
    serverTimezone: UTC
```

#### Cross-Server Not Working
1. Check all servers use same database
2. Verify authentication keys match
3. Check firewall allows port 25580
4. Review logs for authentication errors

#### Performance Issues
```yaml
# Optimize for large networks:
friends:
  suggestions:
    cache-duration: 60  # Increase cache time
database:
  pool-size: 20  # Increase connection pool
debug:
  enabled: false  # Disable debug logging
```


### v1.0.0 (2025-01-20)
- ✨ Initial release
- 🌐 Cross-server friends and party system
- 💾 MySQL and SQLite support
- 🎮 Interactive GUI system
- 🔒 Confirmation system for actions
- 🔧 Comprehensive admin commands
- 📡 Real-time network synchronization

---

**Made with ❤️ for the Minecraft community** 
