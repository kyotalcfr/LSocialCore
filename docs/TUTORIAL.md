# 📚 LSocialCore Complete Tutorial
## 🔧 Prerequisites

### Server Requirements
- **Minecraft Version**: 1.21 or higher
- **Server Software**: Paper, Purpur, or other Paper forks
- **Java Version**: 17 or higher
- **RAM**: Minimum 2GB, recommended 4GB+
- **Storage**: 100MB+ free space

### Network Requirements (Multi-Server)
- **Proxy**: Velocity 3.3+ (recommended) or BungeeCord
- **Database**: MySQL 8.0+ or PostgreSQL 12+
- **Network**: All servers must reach the database
- **Ports**: Default cross-server port 25580 (configurable)

### Purchase & Download LSocialCore

#### Where to Buy
- **🟣 Polymart**: Premium Minecraft resource marketplace with buyer protection
- **🔴 BuiltByBit**: Minecraft development community with verified sellers

#### What's Included
- `LSocialCore-1.0.0-dist.jar` - Main plugin for Paper servers
- `LSocialCore-Velocity.jar` - Velocity proxy plugin (included in purchase)
- Complete documentation and setup guides
- Developer API access
- Support and updates

### Download LSocialCore
1. Visit Polymart or BuiltByBit marketplace
2. Purchase and download `LSocialCore-1.0.0-dist.jar`
3. For Velocity: Download `LSocialCore-Velocity.jar` (included in purchase)

---

## 🏠 Single Server Setup

Perfect for servers that don't need cross-server functionality.

### Step 1: Installation
```bash
# Upload plugin to your server
scp LSocialCore-1.0.0-dist.jar user@yourserver:/path/to/plugins/

# Or use your server panel file manager
# Place file in: /plugins/LSocialCore-1.0.0-dist.jar
```

### Step 2: First Start
```bash
# Start your server
java -Xmx4G -jar paper.jar

# Look for these log messages:
[INFO] [LSocialCore] Plugin enabled successfully!
[INFO] [LSocialCore] Database connection established
[INFO] [LSocialCore] Friends system initialized
[INFO] [LSocialCore] Party system initialized
```

### Step 3: Basic Configuration
The plugin works out-of-the-box with SQLite, but you can customize:

```yaml
# plugins/LSocialCore/config.yml
friends:
  enabled: true
  max-friends: 50          # Max friends per player
  request-timeout: 300     # Friend request timeout (seconds)
  
party:
  enabled: true
  max-party-size: 8        # Max party members
  party-teleport: true     # Allow party teleportation
  
cross-server:
  enabled: false           # Disable for single server
```

### Step 4: Test Basic Functionality
```bash
# In-game commands to test:
/friends                 # Should open friends GUI
/party                   # Should open party GUI
/lsocialcore info        # Should show plugin info
```

**✅ Single server setup complete!**

---

## 🌐 Multi-Server Network Setup

For networks with multiple servers connected via Velocity/BungeeCord.

### Architecture Overview
```
Players → Velocity Proxy → Paper Servers
              ↓
          Shared Database
              ↑
         LSocialCore Data
```

### Step 1: Database Setup

#### Option A: MySQL (Recommended)
```sql
-- Connect to MySQL as root
mysql -u root -p

-- Create database and user
CREATE DATABASE lsocialcore CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'lsocial'@'%' IDENTIFIED BY 'StrongPassword123!';
GRANT ALL PRIVILEGES ON lsocialcore.* TO 'lsocial'@'%';
FLUSH PRIVILEGES;

-- Test connection
mysql -u lsocial -p lsocialcore
```

#### Option B: PostgreSQL
```sql
-- Create database and user
CREATE DATABASE lsocialcore;
CREATE USER lsocial WITH PASSWORD 'StrongPassword123!';
GRANT ALL PRIVILEGES ON DATABASE lsocialcore TO lsocial;
```

### Step 2: Velocity Proxy Setup

#### Install LSocialCore on Velocity
```bash
# Place in Velocity plugins folder
cp LSocialCore-Velocity.jar /velocity/plugins/
```

#### Configure Velocity Plugin
```yaml
# velocity/plugins/lsocialcore/config.yml
cross-server:
  enabled: true
  authentication:
    enabled: true
    auto-key: true              # Auto-generate authentication key
    port: 25580                 # Cross-server communication port
    proxy-host: "0.0.0.0"      # Listen on all interfaces
  
# The plugin will auto-generate secure keys
# Key files will be created in: plugins/lsocialcore/keys/
```

### Step 3: Paper Servers Configuration

#### Install on All Paper Servers
```bash
# Install on each Paper server
cp LSocialCore-1.0.0-dist.jar /server1/plugins/
cp LSocialCore-1.0.0-dist.jar /server2/plugins/
cp LSocialCore-1.0.0-dist.jar /server3/plugins/
```

#### Configure Each Server
```yaml
# server1/plugins/LSocialCore/config.yml
database:
  type: "mysql"
  host: "database.example.com"
  port: 3306
  database: "lsocialcore"
  username: "lsocial"
  password: "StrongPassword123!"
  
cross-server:
  enabled: true
  server-name: "lobby"          # Unique name for this server
  authentication:
    enabled: true
    proxy-host: "proxy.example.com"
    proxy-port: 25580

# server2/plugins/LSocialCore/config.yml
# ... same database config ...
cross-server:
  server-name: "survival"       # Different name for each server

# server3/plugins/LSocialCore/config.yml  
# ... same database config ...
cross-server:
  server-name: "creative"       # Different name for each server
```

### Step 4: Start Network
```bash
# Start in order:
1. Start MySQL/PostgreSQL database
2. Start Velocity proxy
3. Start all Paper servers

# Check logs for these messages:
[Velocity] LSocialCore authentication key generated
[Paper] Cross-server authentication successful
[Paper] Connected to network: 3 servers detected
```

---

## 💾 Database Configuration

### SQLite (Single Server)
```yaml
database:
  type: "sqlite"
  file: "lsocialcore.db"       # Database file location
```

### MySQL (Multi-Server Recommended)
```yaml
database:
  type: "mysql"
  host: "localhost"
  port: 3306
  database: "lsocialcore"
  username: "lsocial"
  password: "your_password"
  
  # Advanced MySQL settings
  properties:
    useSSL: false
    allowPublicKeyRetrieval: true
    serverTimezone: "UTC"
    characterEncoding: "utf8"
    useUnicode: true
    
  # Connection pool settings (for high-traffic servers)
  pool:
    maximum-pool-size: 20
    minimum-idle: 5
    connection-timeout: 30000
    idle-timeout: 600000
    max-lifetime: 1800000
```

### PostgreSQL (Alternative)
```yaml
database:
  type: "postgresql"
  host: "localhost"
  port: 5432
  database: "lsocialcore"
  username: "lsocial"
  password: "your_password"
```

### Database Optimization
```sql
-- MySQL optimizations
SET GLOBAL innodb_buffer_pool_size = 1073741824;  -- 1GB
SET GLOBAL max_connections = 200;

-- Add indexes for better performance
CREATE INDEX idx_friends_player1 ON lsocial_friends(player1_uuid);
CREATE INDEX idx_friends_player2 ON lsocial_friends(player2_uuid);
CREATE INDEX idx_sessions_player ON lsocial_player_sessions(player_uuid);
CREATE INDEX idx_sessions_server ON lsocial_player_sessions(server_name);
```

---

## ⚙️ Plugin Configuration

### Friends System Configuration
```yaml
friends:
  enabled: true
  
  # Limits & Rules
  limits:
    max-friends: 50                    # Max friends per player
    max-pending-requests: 10           # Max pending friend requests
    request-timeout: 300               # Request timeout (5 minutes)
    
  # Cross-Server Features
  cross-server:
    enabled: true                      # Enable cross-server friends
    sync-interval: 30                  # Sync data every 30 seconds
    notification-broadcast: true       # Broadcast friend status changes
    
  # Notifications
  notifications:
    join-messages: true                # "Friend X joined the server"
    leave-messages: true               # "Friend X left the server"
    server-switch: true                # "Friend X switched to Creative"
    friend-request: true               # New friend request notifications
    friend-accept: true                # Friend request accepted notifications
    
  # GUI Settings
  gui:
    enabled: true
    title: "&6&lFriends Menu"
    size: 54
    update-interval: 20                # Update interval in ticks
    
  # Friend Suggestions System
  suggestions:
    enabled: true
    gui-title: "&6&lFriend Suggestions"
    max-suggestions: 45                # Max players to show in suggestions GUI
    cache-duration: 30                 # Seconds to cache network player data
    exclude-same-server: false         # If true, only show players from other servers
    show-server-info: true             # Show which server players are on
```

### Party System Configuration
```yaml
party:
  enabled: true
  
  # Limits & Rules
  limits:
    max-party-size: 8                  # Max party members
    max-invites: 5                     # Max pending invites
    invite-timeout: 60                 # Invite timeout (1 minute)
    
  # Cross-Server Features
  cross-server:
    enabled: true                      # Enable cross-server parties
    auto-follow: true                  # Party members follow leader
    sync-chat: true                    # Party chat across servers
    
  # Party Features
  features:
    auto-accept-friends: false         # Auto-accept friend invites
    leader-only-invite: false          # Only leader can invite
    party-chat: true                   # Dedicated party chat
    party-teleport: true               # /party tp command
    leader-controls: true              # Leader can control party
    cross-server-warp: true            # Warp party to different servers
    
  # Notifications
  notifications:
    party-invite: true                 # Party invite notifications
    party-join: true                   # Member join notifications
    party-leave: true                  # Member leave notifications
    party-kick: true                   # Member kick notifications
    party-disband: true                # Party disband notifications
    
  # GUI Settings
  gui:
    enabled: true
    title: "&5&lParty Menu"
    size: 54
    update-interval: 20                # Update interval in ticks
    
  # Private Party System
  private:
    enabled: true
    allow-password-protection: true
    max-password-length: 16
    password-hash-algorithm: "SHA-256"
    allow-named-parties: true
    max-party-name-length: 32
```

### Cross-Server Configuration
```yaml
cross-server:
  enabled: true
  server-name: "lobby"                 # Unique identifier for this server
  
  # Authentication Settings
  authentication:
    enabled: true                      # Enable key-based authentication
    auto-key: true                     # Auto-generate authentication keys
    secret-key: "auto-generated"       # Leave as auto-generated
    port: 25580                        # Cross-server communication port
    proxy-host: "localhost"            # Velocity proxy address
    proxy-port: 25580                  # Velocity proxy port
    
  # Network Discovery
  discovery:
    enabled: true                      # Enable automatic server discovery
    announcement-interval: 60          # Announce this server every 60 seconds
    timeout: 180                       # Remove servers after 3 minutes offline
    
  # Data Synchronization
  sync:
    friends: true                      # Sync friend data across servers
    parties: true                      # Sync party data across servers
    sessions: true                     # Sync player sessions
    interval: 10                       # Sync interval in seconds
```

---

## ✅ Testing & Verification

### Basic Functionality Tests

#### 1. Single Player Tests
```bash
# Test commands
/friends                     # Should open GUI
/friends help               # Should show help
/party                      # Should open GUI
/lsocialcore info           # Should show plugin info

# Test friend requests (need 2 players)
Player1: /friends add Player2
Player2: /friends accept Player1
Player1: /friends list       # Should show Player2
```

#### 2. Party System Tests
```bash
# Test party creation
Player1: /party create
Player1: /party invite Player2
Player2: /party join Player1
Player1: /party list         # Should show both players

# Test party teleportation
Player2: /party tp Player1   # Should teleport to Player1
Player1: /party tphere       # Should summon Player2
```

#### 3. Cross-Server Tests
```bash
# Player1 on server "lobby", Player2 on server "survival"
Player1: /friends add Player2
# Player2 should receive notification on different server

Player1: /friends             # Should show Player2 as online on "survival"
Player1: /lsocialcore network status  # Should show 2 servers, 2 players
```

### Network Monitoring
```bash
# Admin commands for monitoring
/lsocialcore network status
# Expected output:
# Network Status: 3 servers connected
# Total Players: 25
# Cross-Server: Enabled
# Authentication: Active

/lsocialcore network players
# Expected output:
# lobby: 10 players
# survival: 8 players  
# creative: 7 players

/lsocialcore network sessions
# Should show all active player sessions with timestamps
```

### Database Verification
```sql
-- Check if tables were created
SHOW TABLES;
-- Expected: lsocial_friends, lsocial_friend_requests, lsocial_parties, etc.

-- Check friend data
SELECT * FROM lsocial_friends LIMIT 5;

-- Check active sessions
SELECT * FROM lsocial_player_sessions WHERE is_online = 1;

-- Check party data
SELECT * FROM lsocial_parties;
```

---

## 👥 Player Usage Guide

### Making Friends

#### Using Commands
```bash
# Send friend request
/friends add PlayerName

# Check pending requests  
/friends list

# Accept friend request
/friends accept PlayerName

# Deny friend request
/friends deny PlayerName

# Remove friend
/friends remove PlayerName
```

#### Using GUI
1. Type `/friends` to open Friends Menu
2. Click "Find Friends" for suggestions
3. Click player heads to send friend requests
4. Right-click friend heads to remove friends
5. Left-click friend heads to teleport (with confirmation)

### Party System

#### Creating & Managing Parties
```bash
# Create party
/party create

# Create private party
/party private mypassword123 AwesomeParty

# Invite players
/party invite PlayerName

# Join party
/party join LeaderName
/party join PartyName mypassword123  # For private parties

# Manage party
/party kick PlayerName     # Leader only
/party leave              # Leave party
/party disband            # Leader only
```

#### Party Teleportation
```bash
# Teleport to party member
/party tp MemberName

# Summon all party members (leader only)
/party tphere

# Note: All teleportations require confirmation (double-click)
```

#### Party GUI Usage
1. Type `/party` to open Party Menu
2. Click "Create Party" if not in party
3. Click member heads to teleport
4. Right-click member heads to kick (leader only)
5. Use "Invite Player" to invite new members

---

## 👑 Admin Management

### Server Administration

#### Monitoring Commands
```bash
# Network overview
/lsocialcore network status

# Player distribution
/lsocialcore network players

# Active sessions
/lsocialcore network sessions

# Force refresh network data
/lsocialcore network refresh
```

#### Configuration Management
```bash
# Reload configuration
/lsocialcore reload

# Plugin information
/lsocialcore info

# Help command
/lsocialcore help
```

### Database Management

#### Backup Commands
```sql
-- MySQL backup
mysqldump -u lsocial -p lsocialcore > lsocialcore_backup.sql

-- Restore backup  
mysql -u lsocial -p lsocialcore < lsocialcore_backup.sql
```

#### Cleanup Commands
```sql
-- Remove expired friend requests
DELETE FROM lsocial_friend_requests WHERE expires_at < NOW();

-- Remove old player sessions (older than 7 days)
DELETE FROM lsocial_player_sessions WHERE last_seen < (UNIX_TIMESTAMP() - 604800) * 1000;

-- Remove empty parties
DELETE FROM lsocial_parties WHERE id NOT IN (SELECT DISTINCT party_id FROM lsocial_party_members);
```

### Performance Monitoring
```bash
# Check server logs for performance issues
tail -f logs/latest.log | grep LSocialCore

# Monitor database connections
SHOW PROCESSLIST;  # In MySQL

# Check memory usage
/lsocialcore debug memory  # If debug mode enabled
```

---

## 🔧 Troubleshooting

### Common Issues

#### 1. Plugin Not Loading
**Symptoms**: Plugin doesn't appear in `/plugins` list

**Solutions**:
```bash
# Check Java version
java -version  # Must be 17+

# Check server software
# Must be Paper 1.21+ or compatible fork

# Check plugin file
ls -la plugins/LSocialCore*
# File should be ~18MB

# Check server logs
tail -f logs/latest.log | grep LSocialCore
```

#### 2. Database Connection Failed
**Symptoms**: "Failed to connect to database" in logs

**Solutions**:
```yaml
# Check database configuration
database:
  host: "correct-host"
  port: 3306
  username: "correct-user"
  password: "correct-password"

# Test database connection manually
mysql -h host -P 3306 -u username -p database

# Check firewall rules
telnet database-host 3306

# Try alternative configuration
database:
  properties:
    useSSL: false
    allowPublicKeyRetrieval: true
```

#### 3. Cross-Server Not Working
**Symptoms**: Players can't see friends on other servers

**Solutions**:
```bash
# Check all servers use same database
# Verify in each server's config.yml

# Check authentication keys
/lsocialcore key info  # On Velocity
# Should show "Key: Active, Servers: X connected"

# Check network connectivity
telnet proxy-host 25580

# Verify server names are unique
# Each server must have different server-name in config
```

#### 4. GUI Not Opening
**Symptoms**: `/friends` or `/party` commands don't open GUI

**Solutions**:
```yaml
# Check GUI is enabled
friends:
  gui:
    enabled: true

party:
  gui:
    enabled: true

# Check for plugin conflicts
# Disable other GUI-related plugins temporarily

# Check permissions
# Players need: lsocialcore.friends, lsocialcore.party
```

#### 5. Friend Requests Not Working
**Symptoms**: Friend requests don't send or receive

**Solutions**:
```bash
# Check cooldown
# Wait 3 seconds between requests

# Check friend limits
# Default max: 50 friends, 10 pending requests

# Check player name spelling
# Names are case-sensitive

# Check database
SELECT * FROM lsocial_friend_requests WHERE receiver_uuid = 'player-uuid';
```

### Performance Issues

#### High CPU Usage
```yaml
# Reduce update frequency
friends:
  gui:
    update-interval: 40  # Increase from 20

cross-server:
  sync:
    interval: 30         # Increase from 10

# Disable debug logging
debug:
  enabled: false
```

#### High Memory Usage
```yaml
# Reduce cache duration
friends:
  suggestions:
    cache-duration: 15   # Reduce from 30

# Optimize database pool
database:
  pool:
    maximum-pool-size: 10  # Reduce if needed
    minimum-idle: 2
```

#### Slow Database Queries
```sql
-- Add database indexes
CREATE INDEX idx_friends_lookup ON lsocial_friends(player1_uuid, player2_uuid);
CREATE INDEX idx_sessions_active ON lsocial_player_sessions(is_online, last_seen);
CREATE INDEX idx_requests_receiver ON lsocial_friend_requests(receiver_uuid, expires_at);

-- Optimize MySQL settings
SET GLOBAL query_cache_size = 268435456;  -- 256MB
SET GLOBAL innodb_buffer_pool_size = 1073741824;  -- 1GB
```

### Debug Mode
```yaml
# Enable debug mode for detailed logging
debug:
  enabled: true
  log-database-queries: true
  log-cross-server-events: true

# Check debug logs
tail -f logs/latest.log | grep DEBUG
```

---

## 🚀 Advanced Configuration

### Custom Messages
```yaml
# Create: plugins/LSocialCore/messages.yml
messages:
  friends:
    friend-request-sent: "&aFriend request sent to &e{player}&a!"
    friend-request-received: "&e{player} &awants to be your friend!"
    already-friends: "&cYou are already friends with &e{player}&c!"
    
  party:
    party-created: "&aParty created! Use &e/party invite &ato invite players."
    party-disbanded: "&cParty has been disbanded."
    
  # Custom prefixes
  prefix: "&8[&6LSocial&8] "
  error-prefix: "&8[&cError&8] "
```

### Advanced Database Settings
```yaml
database:
  # Multiple database support
  type: "mysql"
  
  # Master-slave configuration
  master:
    host: "master-db.example.com"
    port: 3306
  
  slaves:
    - host: "slave1-db.example.com"
      port: 3306
    - host: "slave2-db.example.com"
      port: 3306
      
  # Connection pool optimization
  pool:
    maximum-pool-size: 50
    minimum-idle: 10
    connection-timeout: 60000
    idle-timeout: 300000
    max-lifetime: 3600000
    
  # Query optimization
  queries:
    batch-size: 100
    timeout: 30000
    retry-attempts: 3
```

### Custom Permissions
```yaml
# permissions.yml
permissions:
  friends:
    base: "lsocialcore.friends"
    add: "lsocialcore.friends.add"
    remove: "lsocialcore.friends.remove"
    teleport: "lsocialcore.friends.teleport"
    bypass-limit: "lsocialcore.friends.unlimited"
    
  party:
    base: "lsocialcore.party"
    create: "lsocialcore.party.create"
    invite: "lsocialcore.party.invite"
    teleport: "lsocialcore.party.teleport"
    leader: "lsocialcore.party.leader"
    
  admin:
    reload: "lsocialcore.admin.reload"
    debug: "lsocialcore.admin.debug"
    network: "lsocialcore.admin.network"
```

### Webhook Integration
```yaml
# webhooks.yml - Send events to Discord/Slack
webhooks:
  enabled: true
  
  discord:
    enabled: true
    url: "https://discord.com/api/webhooks/..."
    events:
      - friend-request
      - party-create
      - network-status
      
  custom:
    enabled: true
    url: "https://your-api.com/webhook"
    headers:
      Authorization: "Bearer your-token"
      Content-Type: "application/json"
```

### Load Balancing
```yaml
# For very large networks (1000+ concurrent players)
load-balancing:
  enabled: true
  
  # Distribute database load
  database-sharding:
    enabled: true
    shards:
      - name: "shard1"
        servers: ["lobby", "survival"]
        database: "lsocialcore_shard1"
      - name: "shard2"  
        servers: ["creative", "skyblock"]
        database: "lsocialcore_shard2"
        
  # Cache distribution
  redis:
    enabled: true
    host: "redis.example.com"
    port: 6379
    password: "redis-password"
    
  # Regional servers
  regions:
    na-east:
      servers: ["na-lobby", "na-survival"]
      database: "lsocialcore_na"
    eu-west:
      servers: ["eu-lobby", "eu-survival"]  
      database: "lsocialcore_eu"
```

---

## 📊 Monitoring & Analytics

### Built-in Metrics
```bash
# View real-time statistics
/lsocialcore stats

# Export statistics
/lsocialcore export stats.json

# Performance metrics
/lsocialcore performance
```

### External Monitoring
```yaml
# metrics.yml
metrics:
  enabled: true
  
  # Prometheus metrics
  prometheus:
    enabled: true
    port: 9090
    path: "/metrics"
    
  # InfluxDB integration
  influxdb:
    enabled: true
    url: "http://influxdb:8086"
    database: "lsocialcore"
    
  # Custom metrics
  custom:
    - name: "active_friendships"
      query: "SELECT COUNT(*) FROM lsocial_friends"
    - name: "active_parties"
      query: "SELECT COUNT(*) FROM lsocial_parties"
```

---

**🎉 Congratulations! You've completed the LSocialCore tutorial.**
