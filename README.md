# 🚀 LSocialCore - Advanced Friends & Party System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Java](https://img.shields.io/badge/Java-17+-orange.svg)](https://www.oracle.com/java/)
[![Paper](https://img.shields.io/badge/Paper-1.21+-green.svg)](https://papermc.io/)
[![Velocity](https://img.shields.io/badge/Velocity-3.3+-blue.svg)](https://velocitypowered.com/)

**📦 Available on:**
- 🟣 **[Polymart](https://polymart.org)** - Premium Minecraft Resource Marketplace
- 🔴 **[BuiltByBit](https://builtbybit.com)** - Minecraft Development Community

**💻 Source Code:**
- 📂 **[GitHub Repository](https://github.com/kyotalcfr/LSocialCore)** - Documentation, Source Code & Issues

> Professional-grade social plugin for Minecraft networks with advanced cross-server functionality and enterprise-level reliability.

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

### 🎉 Professional Party System
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

1. **Purchase & Download** from Polymart or BuiltByBit
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
1. Install `LSocialCore-1.0.0-dist.jar` on all Paper servers
2. Configure each server with same database:
```