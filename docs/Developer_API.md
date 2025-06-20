# 🔌 LSocialCore Developer API

> Complete API documentation for integrating LSocialCore with your plugins

## 🚀 Getting Started

### Prerequisites
- **Java**: 17 or higher
- **Server**: Paper 1.21+ or Velocity 3.3+
- **LSocialCore**: v1.0.0+

### Code Integration Example

```java
public class YourPlugin extends JavaPlugin {
    private LSocialCore lsocialCore;
    private FriendsManager friendsManager;
    private PartyManager partyManager;
    private NetworkManager networkManager;
    
    @Override
    public void onEnable() {
        if (!setupLSocialCore()) {
            getLogger().severe("LSocialCore not found! Disabling plugin.");
            getServer().getPluginManager().disablePlugin(this);
            return;
        }
        
        getLogger().info("LSocialCore integration enabled!");
        
        // Register your event listeners
        getServer().getPluginManager().registerEvents(new YourListener(lsocialCore), this);
        
        // Initialize your systems
        initializeIntegration();
    }
    
    private boolean setupLSocialCore() {
        // Check if LSocialCore JAR is in classpath (development)
        try {
            Class.forName("com.lythnorb.lsocialcore.LSocialCore");
        } catch (ClassNotFoundException e) {
            getLogger().severe("LSocialCore JAR not found in classpath!");
            getLogger().severe("Please add LSocialCore-1.0.0-dist.jar to your IDE libraries!");
            return false;
        }
        
        // Check if plugin is installed and enabled (runtime)
        Plugin plugin = getServer().getPluginManager().getPlugin("LSocialCore");
        if (!(plugin instanceof LSocialCore)) {
            getLogger().severe("LSocialCore plugin not installed on server!");
            getLogger().severe("Please install LSocialCore-1.0.0-dist.jar to plugins/ folder!");
            return false;
        }
        
        lsocialCore = (LSocialCore) plugin;
        friendsManager = lsocialCore.getFriendsManager();
        partyManager = lsocialCore.getPartyManager();
        networkManager = lsocialCore.getNetworkManager();
        
        return true;
    }
    
    private void initializeIntegration() {
        // Your integration logic here
        getLogger().info("Managers initialized:");
        getLogger().info("- Friends: " + (friendsManager != null ? "✓" : "✗"));
        getLogger().info("- Party: " + (partyManager != null ? "✓" : "✗"));
        getLogger().info("- Network: " + (networkManager != null ? "✓" : "✗"));
    }
    
    // Check if LSocialCore is available
    public boolean hasLSocialCore() {
        return lsocialCore != null;
    }
    
    // Getter methods for easy access
    public FriendsManager getFriendsManager() { return friendsManager; }
    public PartyManager getPartyManager() { return partyManager; }
    public NetworkManager getNetworkManager() { return networkManager; }
}
```

---

## 📦 Project Setup: Manual JAR Integration

### Step 1: Purchase & Download (Development Only)
1. **Purchase** `LSocialCore-1.0.0-dist.jar` from:
   - 🟣 **Polymart**
   - 🔴 **BuiltByBit** 
2. **Create** `libs/` folder in your project root (**DEVELOPMENT ONLY**)
3. **Place** JAR file as `libs/LSocialCore-1.0.0-dist.jar` (**FOR CODING ONLY**)

> ⚠️ **IMPORTANT**: The `libs/` folder is **ONLY for development**. The JAR will **NOT** be included in your final plugin when using `compileOnly`.

### Step 2: IDE Configuration

#### IntelliJ IDEA
1. **File** → **Project Structure** → **Libraries**
2. **Click** `+` → **Java** → **Select** `libs/LSocialCore-1.0.0-dist.jar`
3. **Apply** and **OK**

#### Eclipse
1. **Right-click** project → **Properties**
2. **Java Build Path** → **Libraries** → **Add External JARs**
3. **Select** `libs/LSocialCore-1.0.0-dist.jar`

#### VS Code
1. **Add** to `.classpath` file or workspace settings
2. **Configure** Java extension to include the JAR

### Step 3: Build Configuration

#### Maven (pom.xml)
```xml
<dependency>
    <groupId>com.lythnorb</groupId>
    <artifactId>LSocialCore</artifactId>
    <version>1.0.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/libs/LSocialCore-1.0.0-dist.jar</systemPath>
</dependency>
```

#### Gradle (build.gradle)
```gradle
dependencies {
    compileOnly files('libs/LSocialCore-1.0.0-dist.jar')
}
```

#### Gradle Kotlin DSL (build.gradle.kts)
```kotlin
dependencies {
    compileOnly(files("libs/LSocialCore-1.0.0-dist.jar"))
}
```

### Step 4: Plugin Dependencies
```yaml
# plugin.yml - Required dependency
depend: [LSocialCore]
# or soft dependency for optional integration
softdepend: [LSocialCore]
```

### Step 5: Validation (Recommended)
```java
// Add this validation method to your main plugin class
public boolean validateLSocialCore() {
    // 1. Check if JAR is in classpath (development)
    try {
        Class.forName("com.lythnorb.lsocialcore.LSocialCore");
        getLogger().info("✓ LSocialCore JAR found in classpath");
    } catch (ClassNotFoundException e) {
        getLogger().severe("✗ LSocialCore JAR missing from classpath!");
        getLogger().severe("  → Add LSocialCore-1.0.0-dist.jar to your project libraries");
        return false;
    }
    
    // 2. Check if plugin is loaded (runtime)
    if (!getServer().getPluginManager().isPluginEnabled("LSocialCore")) {
        getLogger().severe("✗ LSocialCore plugin not installed on server!");
        getLogger().severe("  → Install LSocialCore-1.0.0-dist.jar to plugins/ folder");
        return false;
    }
    
    // 3. Check version compatibility
    Plugin plugin = getServer().getPluginManager().getPlugin("LSocialCore");
    String version = plugin.getDescription().getVersion();
    if (!version.startsWith("1.0")) {
        getLogger().warning("⚠ LSocialCore version " + version + " may not be compatible");
        getLogger().warning("  → Recommended: v1.0.0+");
    }
    
    getLogger().info("✓ LSocialCore integration validated successfully");
    return true;
}
```

### Project Structure Example
```
YourPlugin/
├── src/main/java/
│   └── com/yourname/yourplugin/
│       ├── YourPlugin.java
│       └── integration/
│           └── LSocialIntegration.java
├── libs/                              # DEVELOPMENT ONLY - NOT distributed
│   └── LSocialCore-1.0.0-dist.jar   # FOR CODING ONLY - NOT in final JAR
├── build.gradle or pom.xml           # Build configuration
└── src/main/resources/
    └── plugin.yml                    # Plugin dependencies
```

> 📝 **Note**: When you build your plugin, the `libs/` folder and its contents are **NOT included** in the final JAR file when using `compileOnly` configuration.

---

## 🏗️ Core API Classes

### Main Plugin Class
```java
public class LSocialCore extends JavaPlugin {
    
    // Core managers
    public FriendsManager getFriendsManager()
    public PartyManager getPartyManager()
    public NetworkManager getNetworkManager()
    public ConfigManager getConfigManager()
    public DatabaseManager getDatabaseManager()
    
    // GUI and utilities
    public GuiManager getGuiManager()
    public MessageManager getMessageManager()
    
    // Addon system
    public AddonManager getAddonManager()
}
```

### Manager Interfaces
```java
// All managers implement these common methods
public interface Manager {
    void initialize();
    void shutdown();
    boolean isEnabled();
    CompletableFuture<Void> reload();
}
```

---

## 👥 Friends API

### FriendsManager Class
```java
public class FriendsManager {
    
    // Core friendship methods
    public CompletableFuture<Boolean> sendFriendRequest(Player sender, String targetName)
    public CompletableFuture<Boolean> acceptFriendRequest(Player accepter, String requesterName)
    public CompletableFuture<Boolean> denyFriendRequest(Player denier, String requesterName)
    public CompletableFuture<Boolean> removeFriend(Player remover, String friendName)
    
    // Query methods
    public boolean areFriends(UUID player1, UUID player2)
    public Set<Friend> getFriends(UUID playerUuid)
    public List<UUID> getOnlineFriends(UUID playerUuid)
    public int getFriendsCount(UUID playerUuid)
    
    // Request management
    public Set<UUID> getPendingRequests(UUID playerUuid)
    public boolean hasPendingRequest(UUID sender, UUID receiver)
    
    // Cache management
    public void loadFriends(UUID playerUuid)
    public void clearCache(UUID playerUuid)
    public void refreshFriendsCache(UUID playerUuid)
}
```

### Friend Model
```java
public class Friend {
    private final UUID uuid;
    private final String name;
    private final long friendshipDate;
    
    public UUID getUuid()
    public String getName()
    public long getFriendshipDate()
    public Date getFriendshipDateAsDate()
    public boolean isOnline()                    // Requires NetworkManager
    public String getCurrentServer()             // Requires NetworkManager
}
```

### Basic Friends API Usage
```java
public class FriendsIntegration {
    private final FriendsManager friendsManager;
    
    public FriendsIntegration(LSocialCore lsocialCore) {
        this.friendsManager = lsocialCore.getFriendsManager();
    }
    
    // Check if players are friends
    public boolean arePlayersFriends(Player player1, Player player2) {
        return friendsManager.areFriends(player1.getUniqueId(), player2.getUniqueId());
    }
    
    // Get all friends of a player
    public Set<Friend> getPlayerFriends(Player player) {
        return friendsManager.getFriends(player.getUniqueId());
    }
    
    // Get only online friends
    public List<UUID> getOnlineFriends(Player player) {
        return friendsManager.getOnlineFriends(player.getUniqueId());
    }
    
    // Send friend request programmatically
    public void sendFriendRequest(Player sender, String targetName) {
        friendsManager.sendFriendRequest(sender, targetName).thenAccept(success -> {
            if (success) {
                sender.sendMessage("§aFriend request sent to " + targetName + "!");
            } else {
                sender.sendMessage("§cFailed to send friend request.");
            }
        }).exceptionally(throwable -> {
            sender.sendMessage("§cError: " + throwable.getMessage());
            return null;
        });
    }
    
    // Check friendship before allowing actions
    public boolean canInteractWithPlayer(Player actor, Player target) {
        // Only allow interaction between friends
        return friendsManager.areFriends(actor.getUniqueId(), target.getUniqueId());
    }
}
```

### Advanced Friends API
```java
public class AdvancedFriendsIntegration {
    
    // Get friends count with async handling
    public CompletableFuture<Integer> getFriendsCountAsync(UUID playerUuid) {
        return CompletableFuture.supplyAsync(() -> {
            return friendsManager.getFriendsCount(playerUuid);
        });
    }
    
    // Get mutual friends between two players
    public Set<Friend> getMutualFriends(UUID player1, UUID player2) {
        Set<Friend> friends1 = friendsManager.getFriends(player1);
        Set<Friend> friends2 = friendsManager.getFriends(player2);
        
        return friends1.stream()
            .filter(friend -> friends2.stream()
                .anyMatch(f -> f.getUuid().equals(friend.getUuid())))
            .collect(Collectors.toSet());
    }
    
    // Check if player can add more friends
    public boolean canAddMoreFriends(UUID playerUuid) {
        int currentFriends = friendsManager.getFriendsCount(playerUuid);
        int maxFriends = lsocialCore.getConfigManager().getMaxFriends();
        return currentFriends < maxFriends;
    }
    
    // Get friend suggestions (players not yet friends)
    public CompletableFuture<List<Player>> getFriendSuggestions(UUID playerUuid) {
        return lsocialCore.getNetworkManager().getNetworkPlayers().thenApply(networkPlayers -> {
            Set<Friend> currentFriends = friendsManager.getFriends(playerUuid);
            Set<UUID> friendUuids = currentFriends.stream()
                .map(Friend::getUuid)
                .collect(Collectors.toSet());
                
            return networkPlayers.stream()
                .filter(playerInfo -> !friendUuids.contains(UUID.fromString(playerInfo.getUuid())))
                .filter(playerInfo -> !playerInfo.getUuid().equals(playerUuid.toString()))
                .map(playerInfo -> Bukkit.getPlayer(playerInfo.getName()))
                .filter(Objects::nonNull)
                .collect(Collectors.toList());
        });
    }
}
```

---

## 🎉 Party API

### PartyManager Class
```java
public class PartyManager {
    
    // Party creation and management
    public CompletableFuture<Boolean> createParty(UUID leaderUuid, String leaderName)
    public CompletableFuture<Boolean> createPrivateParty(UUID leaderUuid, String leaderName, String partyName, String password)
    public CompletableFuture<Boolean> disbandParty(UUID leaderUuid)
    
    // Member management
    public CompletableFuture<Boolean> inviteToParty(UUID leaderUuid, String leaderName, UUID targetUuid, String targetName)
    public CompletableFuture<Boolean> joinParty(UUID playerUuid, String playerName, UUID leaderUuid)
    public CompletableFuture<Boolean> joinPrivateParty(UUID playerUuid, String playerName, String partyName, String password)
    public CompletableFuture<Boolean> leaveParty(UUID playerUuid)
    public CompletableFuture<Boolean> kickFromParty(UUID leaderUuid, UUID targetUuid)
    
    // Query methods
    public boolean isInParty(UUID playerUuid)
    public boolean isPartyLeader(UUID playerUuid)
    public UUID getPartyLeader(UUID playerUuid)
    public Set<UUID> getPartyMembers(UUID playerUuid)
    public List<UUID> getOnlinePartyMembers(UUID playerUuid)
    public int getPartySize(UUID playerUuid)
    public String getPartyName(UUID playerUuid)
    
    // Invite management
    public Set<UUID> getPendingInvites(UUID playerUuid)
    public boolean hasPartyInvite(UUID playerUuid, UUID fromUuid)
    
    // Party settings
    public boolean isPrivateParty(UUID playerUuid)
    public boolean hasPartyPassword(UUID playerUuid)
}
```

### Basic Party API Usage
```java
public class PartyIntegration {
    private final PartyManager partyManager;
    
    public PartyIntegration(LSocialCore lsocialCore) {
        this.partyManager = lsocialCore.getPartyManager();
    }
    
    // Check if player is in party
    public boolean isPlayerInParty(Player player) {
        return partyManager.isInParty(player.getUniqueId());
    }
    
    // Get party members
    public Set<UUID> getPartyMembers(Player player) {
        if (!isPlayerInParty(player)) {
            return Collections.emptySet();
        }
        return partyManager.getPartyMembers(player.getUniqueId());
    }
    
    // Create party for player
    public void createPartyForPlayer(Player player) {
        partyManager.createParty(player.getUniqueId(), player.getName()).thenAccept(success -> {
            if (success) {
                player.sendMessage("§aParty created! Use /party invite to invite players.");
            } else {
                player.sendMessage("§cFailed to create party. You might already be in one.");
            }
        });
    }
    
    // Check if player is party leader
    public boolean isPartyLeader(Player player) {
        return partyManager.isPartyLeader(player.getUniqueId());
    }
    
    // Get party size
    public int getPartySize(Player player) {
        if (!isPlayerInParty(player)) {
            return 0;
        }
        return partyManager.getPartySize(player.getUniqueId());
    }
}
```

---

## 🌐 Network API

### NetworkManager Class
```java
public class NetworkManager {
    
    // Network player information
    public CompletableFuture<List<PlayerInfo>> getNetworkPlayers()
    public int getNetworkPlayerCount()
    public Map<String, Integer> getServerPlayerCounts()
    
    // Player session management
    public void updatePlayerSession(String playerUuid, String playerName, boolean isOnline)
    public PlayerInfo getPlayerInfo(String playerUuid)
    public boolean isPlayerOnline(String playerUuid)
    public String getPlayerServer(String playerUuid)
    
    // Network utilities
    public void refreshNetworkData()
    public String getCurrentServerName()
    public Set<String> getConnectedServers()
    public boolean isCrossServerEnabled()
    
    // Inner class for player information
    public static class PlayerInfo {
        public String getUuid()
        public String getName()
        public String getServer()
        public long getLastSeen()
        public boolean isOnline()
    }
}
```

### Network API Usage
```java
public class NetworkIntegration {
    private final NetworkManager networkManager;
    
    public NetworkIntegration(LSocialCore lsocialCore) {
        this.networkManager = lsocialCore.getNetworkManager();
    }
    
    // Get total network players
    public int getTotalNetworkPlayers() {
        return networkManager.getNetworkPlayerCount();
    }
    
    // Get player distribution across servers
    public Map<String, Integer> getServerDistribution() {
        return networkManager.getServerPlayerCounts();
    }
    
    // Check if specific player is online on network
    public boolean isPlayerOnlineAnywhere(String playerUuid) {
        return networkManager.isPlayerOnline(playerUuid);
    }
    
    // Get which server a player is on
    public String getPlayerCurrentServer(String playerUuid) {
        return networkManager.getPlayerServer(playerUuid);
    }
    
    // Get all connected servers
    public Set<String> getNetworkServers() {
        return networkManager.getConnectedServers();
    }
    
    // Get detailed network information
    public CompletableFuture<String> getNetworkReport() {
        return networkManager.getNetworkPlayers().thenApply(players -> {
            StringBuilder report = new StringBuilder();
            report.append("=== Network Report ===\n");
            report.append("Total Players: ").append(players.size()).append("\n");
            
            Map<String, Integer> serverCounts = networkManager.getServerPlayerCounts();
            report.append("Server Distribution:\n");
            serverCounts.forEach((server, count) -> 
                report.append("  ").append(server).append(": ").append(count).append(" players\n"));
                
            return report.toString();
        });
    }
}
```

---

## 📡 Events API

### Available Events
```java
// Friend-related events
public class FriendRequestEvent extends PlayerEvent implements Cancellable
public class FriendAcceptEvent extends PlayerEvent  
public class FriendRemoveEvent extends PlayerEvent
public class FriendJoinNetworkEvent extends PlayerEvent
public class FriendLeaveNetworkEvent extends PlayerEvent

// Party-related events
public class PartyCreateEvent extends PlayerEvent implements Cancellable
public class PartyDisbandEvent extends PlayerEvent
public class PartyJoinEvent extends PlayerEvent implements Cancellable
public class PartyLeaveEvent extends PlayerEvent
public class PartyKickEvent extends PlayerEvent implements Cancellable
public class PartyInviteEvent extends PlayerEvent implements Cancellable

// Network events
public class PlayerJoinNetworkEvent extends PlayerEvent
public class PlayerLeaveNetworkEvent extends PlayerEvent
public class ServerConnectEvent extends Event
public class ServerDisconnectEvent extends Event
```

### Event Examples
```java
public class LSocialEventListener implements Listener {
    
    @EventHandler
    public void onFriendRequest(FriendRequestEvent event) {
        Player sender = event.getSender();
        Player receiver = event.getReceiver();
        
        // Check if players are in same faction (example)
        if (!isSameFaction(sender, receiver)) {
            event.setCancelled(true);
            sender.sendMessage("§cYou can only befriend players in your faction!");
            return;
        }
        
        // Give reward for sending friend request
        giveExperience(sender, 10);
    }
    
    @EventHandler
    public void onFriendAccept(FriendAcceptEvent event) {
        Player player1 = event.getPlayer1();
        Player player2 = event.getPlayer2();
        
        // Broadcast to server
        Bukkit.broadcastMessage("§e" + player1.getName() + " §aand §e" + player2.getName() + " §aare now friends!");
        
        // Give rewards to both players
        giveReward(player1, "friend_bonus");
        giveReward(player2, "friend_bonus");
        
        // Update statistics
        updatePlayerStat(player1, "friends_made", 1);
        updatePlayerStat(player2, "friends_made", 1);
    }
    
    @EventHandler  
    public void onPartyCreate(PartyCreateEvent event) {
        Player leader = event.getLeader();
        
        // Check if player has permission to create parties
        if (!leader.hasPermission("myplugin.party.create")) {
            event.setCancelled(true);
            leader.sendMessage("§cYou don't have permission to create parties!");
            return;
        }
        
        // Give party leader special effects
        leader.addPotionEffect(new PotionEffect(PotionEffectType.REGENERATION, 6000, 0));
        leader.sendMessage("§aParty created! You received regeneration as party leader.");
    }
    
    @EventHandler
    public void onPartyJoin(PartyJoinEvent event) {
        Player player = event.getPlayer();
        UUID partyId = event.getPartyId();
        
        // Apply party buffs to all members
        Set<UUID> members = partyManager.getPartyMembers(player.getUniqueId());
        for (UUID memberUuid : members) {
            Player member = Bukkit.getPlayer(memberUuid);
            if (member != null) {
                applyPartyBuff(member);
            }
        }
        
        // Announce to party
        broadcastToParty(partyId, "§e" + player.getName() + " §ajoined the party!");
    }
    
    @EventHandler
    public void onPlayerJoinNetwork(PlayerJoinNetworkEvent event) {
        Player player = event.getPlayer();
        String serverName = event.getServerName();
        
        // Notify friends about server switch
        Set<Friend> friends = friendsManager.getFriends(player.getUniqueId());
        for (Friend friend : friends) {
            Player friendPlayer = Bukkit.getPlayer(friend.getUuid());
            if (friendPlayer != null) {
                friendPlayer.sendMessage("§e" + player.getName() + " §ajoined §e" + serverName);
            }
        }
    }
}
```

---

## 🔗 Integration Examples

### Dungeon Plugin Integration
```java
public class DungeonPartyIntegration {
    private final LSocialCore lsocialCore;
    private final PartyManager partyManager;
    private final FriendsManager friendsManager;
    
    public DungeonPartyIntegration(LSocialCore lsocialCore) {
        this.lsocialCore = lsocialCore;
        this.partyManager = lsocialCore.getPartyManager();
        this.friendsManager = lsocialCore.getFriendsManager();
    }
    
    // Start dungeon with party support
    public boolean startDungeon(Player leader, String dungeonName) {
        if (partyManager.isInParty(leader.getUniqueId())) {
            return startPartyDungeon(leader, dungeonName);
        } else {
            return startSoloDungeon(leader, dungeonName);
        }
    }
    
    private boolean startPartyDungeon(Player leader, String dungeonName) {
        // Get all party members
        Set<UUID> partyMembers = partyManager.getPartyMembers(leader.getUniqueId());
        List<UUID> onlineMembers = partyManager.getOnlinePartyMembers(leader.getUniqueId());
        
        // Check party size limits
        DungeonConfig config = getDungeonConfig(dungeonName);
        if (onlineMembers.size() < config.getMinPlayers() || 
            onlineMembers.size() > config.getMaxPlayers()) {
            leader.sendMessage("§cParty size must be between " + config.getMinPlayers() + 
                             " and " + config.getMaxPlayers() + " players!");
            return false;
        }
        
        // Check if all members meet requirements
        for (UUID memberUuid : onlineMembers) {
            Player member = Bukkit.getPlayer(memberUuid);
            if (member == null || !meetsDungeonRequirements(member, dungeonName)) {
                leader.sendMessage("§cAll party members must meet dungeon requirements!");
                return false;
            }
        }
        
        // Start dungeon for entire party
        DungeonInstance instance = createDungeonInstance(dungeonName, onlineMembers);
        
        // Teleport all party members
        Location spawnLocation = instance.getSpawnLocation();
        for (UUID memberUuid : onlineMembers) {
            Player member = Bukkit.getPlayer(memberUuid);
            if (member != null) {
                member.teleport(spawnLocation);
                member.sendMessage("§aWelcome to " + dungeonName + "! Work together with your party!");
            }
        }
        
        // Apply party bonuses
        applyPartyDungeonBonuses(instance, onlineMembers);
        
        return true;
    }
    
    // Party buff system for dungeons
    private void applyPartyDungeonBonuses(DungeonInstance instance, List<UUID> members) {
        // Calculate bonuses based on party size and friendship
        double experienceBonus = 1.0 + (members.size() * 0.1); // 10% per member
        double friendshipBonus = 1.0;
        
        // Check friendships within party for bonus
        int friendships = 0;
        for (int i = 0; i < members.size(); i++) {
            for (int j = i + 1; j < members.size(); j++) {
                if (friendsManager.areFriends(members.get(i), members.get(j))) {
                    friendships++;
                }
            }
        }
        
        if (friendships > 0) {
            friendshipBonus = 1.0 + (friendships * 0.05); // 5% per friendship
        }
        
        // Apply bonuses to dungeon instance
        instance.setExperienceMultiplier(experienceBonus * friendshipBonus);
        instance.setLootMultiplier(Math.min(2.0, 1.0 + (friendships * 0.1))); // Max 2x loot
        
        // Notify party about bonuses
        String bonusMessage = String.format("§aParty bonuses: §e%.1fx EXP, %.1fx Loot", 
                                           experienceBonus * friendshipBonus, 
                                           instance.getLootMultiplier());
        
        for (UUID memberUuid : members) {
            Player member = Bukkit.getPlayer(memberUuid);
            if (member != null) {
                member.sendMessage(bonusMessage);
            }
        }
    }
    
    // Handle party member disconnect in dungeon
    @EventHandler
    public void onPartyMemberLeave(PartyLeaveEvent event) {
        Player player = event.getPlayer();
        DungeonInstance instance = getDungeonInstance(player);
        
        if (instance != null && instance.isActive()) {
            // Handle party member leaving during dungeon
            Set<UUID> remainingMembers = partyManager.getPartyMembers(event.getPartyLeader());
            
            if (remainingMembers.size() < instance.getMinPlayers()) {
                // Not enough players, give grace period
                instance.startGracePeriod(60); // 60 seconds to get new member
                broadcastToDungeon(instance, "§cParty member left! You have 60 seconds to invite someone or the dungeon will end.");
            }
        }
    }
}
```

### Economy Integration
```java
public class EconomyIntegration {
    private final LSocialCore lsocialCore;
    private final FriendsManager friendsManager;
    private final PartyManager partyManager;
    
    // Friend-based discounts
    public double getFriendDiscount(Player buyer, Player shopOwner) {
        if (friendsManager.areFriends(buyer.getUniqueId(), shopOwner.getUniqueId())) {
            return 0.10; // 10% friend discount
        }
        return 0.0;
    }
    
    // Party pooled purchases
    public boolean purchaseForParty(Player buyer, String itemId, double cost) {
        if (!partyManager.isInParty(buyer.getUniqueId())) {
            return false;
        }
        
        Set<UUID> partyMembers = partyManager.getPartyMembers(buyer.getUniqueId());
        double costPerMember = cost / partyMembers.size();
        
        // Check if all members can afford
        for (UUID memberUuid : partyMembers) {
            OfflinePlayer member = Bukkit.getOfflinePlayer(memberUuid);
            if (getPlayerBalance(member) < costPerMember) {
                buyer.sendMessage("§c" + member.getName() + " cannot afford their share!");
                return false;
            }
        }
        
        // Charge all members
        for (UUID memberUuid : partyMembers) {
            OfflinePlayer member = Bukkit.getOfflinePlayer(memberUuid);
            withdrawFromPlayer(member, costPerMember);
        }
        
        // Give item to buyer
        giveItem(buyer, itemId);
        
        // Notify party
        broadcastToParty(buyer.getUniqueId(), 
            "§a" + buyer.getName() + " purchased " + itemId + " for the party! Cost split: $" + costPerMember + " each");
        
        return true;
    }
    
    // Friend referral system
    public void handleFriendReferral(Player referrer, Player newPlayer) {
        if (friendsManager.areFriends(referrer.getUniqueId(), newPlayer.getUniqueId())) {
            // Give bonus to both players
            depositToPlayer(referrer, 1000);
            depositToPlayer(newPlayer, 500);
            
            referrer.sendMessage("§aThanks for referring " + newPlayer.getName() + "! You received $1000!");
            newPlayer.sendMessage("§aWelcome! Your friend " + referrer.getName() + " earned you $500!");
        }
    }
}
```

### PvP Integration
```java
public class PvPIntegration {
    private final LSocialCore lsocialCore;
    private final FriendsManager friendsManager;
    private final PartyManager partyManager;
    
    @EventHandler
    public void onPlayerDamage(EntityDamageByEntityEvent event) {
        if (!(event.getEntity() instanceof Player victim)) return;
        if (!(event.getDamager() instanceof Player attacker)) return;
        
        // Prevent friendly fire between friends
        if (friendsManager.areFriends(attacker.getUniqueId(), victim.getUniqueId())) {
            event.setCancelled(true);
            attacker.sendMessage("§cYou cannot attack your friend!");
            return;
        }
        
        // Prevent party friendly fire
        if (partyManager.isInParty(attacker.getUniqueId()) && 
            partyManager.isInParty(victim.getUniqueId())) {
            
            Set<UUID> attackerParty = partyManager.getPartyMembers(attacker.getUniqueId());
            if (attackerParty.contains(victim.getUniqueId())) {
                event.setCancelled(true);
                attacker.sendMessage("§cYou cannot attack your party member!");
                return;
            }
        }
    }
    
    // Party vs Party battles
    public void startPartyBattle(Player leader1, Player leader2) {
        if (!partyManager.isPartyLeader(leader1.getUniqueId()) || 
            !partyManager.isPartyLeader(leader2.getUniqueId())) {
            return;
        }
        
        Set<UUID> party1 = partyManager.getPartyMembers(leader1.getUniqueId());
        Set<UUID> party2 = partyManager.getPartyMembers(leader2.getUniqueId());
        
        // Create battle arena
        PvPArena arena = createBattleArena(party1.size() + party2.size());
        
        // Teleport parties to opposite sides
        Location spawn1 = arena.getTeamSpawn(1);
        Location spawn2 = arena.getTeamSpawn(2);
        
        for (UUID memberUuid : party1) {
            Player member = Bukkit.getPlayer(memberUuid);
            if (member != null) {
                member.teleport(spawn1);
                member.sendMessage("§cParty Battle! Defeat the enemy party!");
            }
        }
        
        for (UUID memberUuid : party2) {
            Player member = Bukkit.getPlayer(memberUuid);
            if (member != null) {
                member.teleport(spawn2);
                member.sendMessage("§cParty Battle! Defeat the enemy party!");
            }
        }
        
        arena.startBattle(party1, party2);
    }
}
```

---

## ⚡ Best Practices

### 1. Manual JAR Setup Tips
```java
// Always check classpath and plugin availability
public boolean validateLSocialCore() {
    // 1. Check if classes are available in classpath (development)
    try {
        Class.forName("com.lythnorb.lsocialcore.LSocialCore");
    } catch (ClassNotFoundException e) {
        getLogger().severe("LSocialCore JAR missing from classpath!");
        return false;
    }
    
    // 2. Check if plugin is enabled on server (runtime)
    if (!getServer().getPluginManager().isPluginEnabled("LSocialCore")) {
        getLogger().severe("LSocialCore plugin not installed on server!");
        return false;
    }
    
    // 3. Check API version compatibility (recommended)
    Plugin plugin = getServer().getPluginManager().getPlugin("LSocialCore");
    String version = plugin.getDescription().getVersion();
    if (!isCompatibleVersion(version)) {
        getLogger().warning("LSocialCore version " + version + " might not be compatible!");
    }
    
    return true;
}

private boolean isCompatibleVersion(String version) {
    // Check if version is compatible with your plugin
    return version.startsWith("1.0"); // Example: accept 1.0.x versions
}
```

### 2. Async Operations
```java
// Always use async for database operations
public void checkFriendshipAsync(Player player1, Player player2, Consumer<Boolean> callback) {
    CompletableFuture.supplyAsync(() -> {
        return friendsManager.areFriends(player1.getUniqueId(), player2.getUniqueId());
    }).thenAccept(callback).exceptionally(throwable -> {
        plugin.getLogger().severe("Error checking friendship: " + throwable.getMessage());
        callback.accept(false);
        return null;
    });
}
```

### 2. Error Handling
```java
public void handleFriendRequest(Player sender, String targetName) {
    friendsManager.sendFriendRequest(sender, targetName)
        .thenAccept(success -> {
            if (success) {
                sender.sendMessage("§aFriend request sent!");
            } else {
                sender.sendMessage("§cFailed to send friend request.");
            }
        })
        .exceptionally(throwable -> {
            plugin.getLogger().warning("Error sending friend request: " + throwable.getMessage());
            sender.sendMessage("§cAn error occurred while sending the friend request.");
            return null;
        });
}
```

### 3. Cache Usage
```java
// Cache frequently accessed data
private final Map<UUID, Set<Friend>> friendsCache = new ConcurrentHashMap<>();

public Set<Friend> getCachedFriends(UUID playerUuid) {
    return friendsCache.computeIfAbsent(playerUuid, uuid -> {
        // Load from database if not cached
        return friendsManager.getFriends(uuid);
    });
}

// Clear cache when data changes
@EventHandler
public void onFriendAccept(FriendAcceptEvent event) {
    friendsCache.remove(event.getPlayer1().getUniqueId());
    friendsCache.remove(event.getPlayer2().getUniqueId());
}
```

### 4. Event Priority
```java
// Use appropriate event priorities
@EventHandler(priority = EventPriority.HIGH)
public void onFriendRequest(FriendRequestEvent event) {
    // High priority for important checks
}

@EventHandler(priority = EventPriority.MONITOR, ignoreCancelled = true)
public void onFriendAcceptMonitor(FriendAcceptEvent event) {
    // Monitor for logging/statistics
}
```

### 5. Resource Cleanup
```java
public class YourPlugin extends JavaPlugin {
    private final Set<BukkitTask> activeTasks = new HashSet<>();
    
    @Override
    public void onDisable() {
        // Cancel all tasks
        activeTasks.forEach(BukkitTask::cancel);
        activeTasks.clear();
        
        // Clear caches
        friendsCache.clear();
    }
}
```

---

## 🔥 Advanced Usage

### Custom Event Creation
```java
public class CustomSocialEvent extends Event {
    private static final HandlerList HANDLERS = new HandlerList();
    private final Player player;
    private final String eventType;
    private boolean cancelled = false;
    
    public CustomSocialEvent(Player player, String eventType) {
        this.player = player;
        this.eventType = eventType;
    }
    
    public Player getPlayer() { return player; }
    public String getEventType() { return eventType; }
    
    @Override
    public HandlerList getHandlers() { return HANDLERS; }
    public static HandlerList getHandlerList() { return HANDLERS; }
}

// Fire custom events
Bukkit.getPluginManager().callEvent(new CustomSocialEvent(player, "friend_milestone"));
```

### Performance Monitoring
```java
public class PerformanceMonitor {
    private final Map<String, Long> operationTimes = new ConcurrentHashMap<>();
    
    public <T> CompletableFuture<T> monitorOperation(String operation, Supplier<CompletableFuture<T>> task) {
        long startTime = System.currentTimeMillis();
        
        return task.get().whenComplete((result, throwable) -> {
            long duration = System.currentTimeMillis() - startTime;
            operationTimes.put(operation, duration);
            
            if (duration > 1000) { // Log slow operations
                plugin.getLogger().warning("Slow operation detected: " + operation + " took " + duration + "ms");
            }
        });
    }
    
    public Map<String, Long> getOperationTimes() {
        return new HashMap<>(operationTimes);
    }
}
```

### Addon Development
```java
public class YourAddon extends LSocialAddon {
    
    @Override
    public void onLoad() {
        getLogger().info("YourAddon loaded!");
    }
    
    @Override
    public void onEnable() {
        // Register your listeners
        registerListener(new YourListener());
        
        // Register your commands
        registerCommand("youraddon", new YourCommand());
        
        // Access LSocialCore managers
        FriendsManager friendsManager = getLSocialCore().getFriendsManager();
        PartyManager partyManager = getLSocialCore().getPartyManager();
    }
    
    @Override
    public void onDisable() {
        getLogger().info("YourAddon disabled!");
    }
    
    @Override
    public AddonDescription getAddonDescription() {
        return AddonDescription.builder()
            .name("YourAddon")
            .version("1.0.0")
            .author("YourName")
            .description("Your addon description")
            .build();
    }
}
```

---

## 📊 API Reference Summary

### Core Classes
| Class | Purpose | Key Methods |
|-------|---------|-------------|
| `LSocialCore` | Main plugin class | `getFriendsManager()`, `getPartyManager()`, `getNetworkManager()` |
| `FriendsManager` | Friends system | `areFriends()`, `getFriends()`, `sendFriendRequest()` |
| `PartyManager` | Party system | `isInParty()`, `getPartyMembers()`, `createParty()` |
| `NetworkManager` | Cross-server | `getNetworkPlayers()`, `getNetworkPlayerCount()` |

### Event Classes
| Event | Trigger | Cancellable |
|-------|---------|-------------|
| `FriendRequestEvent` | Friend request sent | ✅ |
| `FriendAcceptEvent` | Friend request accepted | ❌ |
| `PartyCreateEvent` | Party created | ✅ |
| `PartyJoinEvent` | Player joins party | ✅ |

### Async Methods
All database operations return `CompletableFuture<T>` for async handling:
- `sendFriendRequest()` → `CompletableFuture<Boolean>`
- `createParty()` → `CompletableFuture<Boolean>`
- `getNetworkPlayers()` → `CompletableFuture<List<PlayerInfo>>`

---

## ⚠️ IMPORTANT: Distribution Guidelines

### ❌ DO NOT Include LSocialCore in Your Plugin JAR

**NEVER** include LSocialCore JAR inside your plugin's shaded/fat JAR. This will cause:

#### Technical Issues:
- 🚨 **Plugin Conflicts** - Server will have duplicate LSocialCore classes
- 🚨 **ClassLoader Errors** - JVM cannot load same classes twice
- 🚨 **Server Crashes** - Bukkit/Paper will fail to start
- 🚨 **Unpredictable Behavior** - Methods may call wrong class instances

#### Legal Issues:
- 🚨 **License Violation** - Redistributing premium plugin without permission
- 🚨 **Copyright Infringement** - LSocialCore is copyrighted by LythNorb
- 🚨 **Marketplace Violation** - Against Polymart/BuiltByBit terms of service
- 🚨 **Account Suspension** - Your marketplace accounts may be banned

### ✅ Correct Integration Method:

```gradle
// ✅ CORRECT - Compile-only dependency
dependencies {
    compileOnly files('libs/LSocialCore-1.0.0-dist.jar')
    compileOnly 'io.papermc.paper:paper-api:1.21.3-R0.1-SNAPSHOT'
}

// ❌ WRONG - Never do this!
dependencies {
    implementation files('libs/LSocialCore-1.0.0-dist.jar') // DON'T!
    shadow files('libs/LSocialCore-1.0.0-dist.jar')         // DON'T!
}
```

```xml
<!-- ✅ CORRECT - System dependency with provided scope -->
<dependency>
    <groupId>com.lythnorb</groupId>
    <artifactId>LSocialCore</artifactId>
    <version>1.0.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/libs/LSocialCore-1.0.0-dist.jar</systemPath>
</dependency>

<!-- ❌ WRONG - Never use compile/runtime scope! -->
<dependency>
    <groupId>com.lythnorb</groupId>
    <artifactId>LSocialCore</artifactId>
    <version>1.0.0</version>
    <scope>compile</scope> <!-- DON'T! -->
</dependency>
```

### ✅ What Your Users Need:

**IMPORTANT**: Your plugin JAR will **NOT** contain LSocialCore. Users must install both plugins separately:

1. **Purchase** LSocialCore separately from Polymart/BuiltByBit
2. **Install** LSocialCore-1.0.0-dist.jar to server's plugins/ folder
3. **Install** your plugin JAR to server's plugins/ folder  
4. **Restart** server

### 🔍 How to Verify Your Build is Correct:

After building your plugin, verify that LSocialCore is **NOT** included:

```bash
# Extract and check your plugin JAR contents
jar -tf YourPlugin.jar | grep -i lsocial
# Should return NOTHING - no LSocialCore classes

# Or check JAR size - should be small without LSocialCore
ls -lh YourPlugin.jar
```

If you see LSocialCore classes in your JAR, you have **WRONG** build configuration!

### ✅ Document This in Your Plugin README:

```markdown
## Requirements
- LSocialCore v1.0.0+ (Purchase from Polymart/BuiltByBit)
- Paper 1.21+ server

## Installation
1. Purchase and download LSocialCore from Polymart or BuiltByBit
2. Place LSocialCore-1.0.0-dist.jar in your plugins/ folder
3. Place YourPlugin.jar in your plugins/ folder
4. Restart your server
```
