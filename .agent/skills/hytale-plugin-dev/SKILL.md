---
name: hytale-plugin-dev
description: Java plugin development for Hytale servers. Covers Java 25 setup, Gradle builds, Antigravity/IDE configuration, plugin lifecycle (setup/start/shutdown), command registration, event handling, ECS architecture, and deployment. Use when creating server plugins, extending game functionality, or implementing custom game mechanics.
---

# Hytale Plugin Development

Create server-side plugins for Hytale using Java 25 and Gradle.

## Overview

Hytale uses a **server-side first** modding approach. All plugins run on the server - players don't need to install anything to join modded servers.

**Tech Stack**:
- **Java 25** - Required JDK version
- **Gradle 9.2** - Build system
- **Antigravity** - Recommended IDE (AI-assisted development!)

## First-Time Setup Flow

When a user asks to create a plugin, guide them through this:

### Step 1: Prerequisites Check

```
I'll help you create that plugin! First, let me verify your setup:

Prerequisites:
1. Java 25 (download from Adoptium or Oracle)
2. Gradle (I can run commands for you in Antigravity!)

If you're using Antigravity, you're all set - I can handle builds directly!
```

### Step 2: Install Java 25

**Windows**:
```powershell
# Download from Adoptium
winget install EclipseAdoptium.Temurin.25.JDK
```

**Or download manually** from: https://adoptium.net/

### Step 3: Create Project from Template

The easiest way to start - use the community template:

```bash
# Clone the official template (Darkhax/Jared)
git clone https://github.com/Darkhax-Hytale/HytalePluginTemplate.git MyPlugin
cd MyPlugin
```

Or create manually with this structure:

```
MyPlugin/
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
└── src/
    └── main/
        └── java/
            └── com/
                └── yourname/
                    └── myplugin/
                        └── MyPlugin.java
```

### Step 4: Open in Antigravity

1. Open the project folder in Antigravity
2. I'll automatically detect the Gradle project
3. Use `gradle build` via run_command to build
4. I can help you write, debug, and test code!

**Alternative: IntelliJ IDEA**
1. Open IntelliJ IDEA
2. **File → Open** → Select project folder
3. **File → Project Structure → Project → SDK** → Select Java 25
4. Let Gradle sync complete

---

## Plugin Lifecycle

Every plugin extends `JavaPlugin` with these lifecycle methods:

```java
package com.yourname.myplugin;

import com.hytale.server.plugin.JavaPlugin;

public class MyPlugin extends JavaPlugin {
    
    @Override
    public void setup() {
        // Called during server setup phase
        // Register commands, events, configs here
        getLogger().info("MyPlugin setting up...");
    }
    
    @Override
    public void start() {
        // Called when server starts
        // Initialize runtime features
        getLogger().info("MyPlugin started!");
    }
    
    @Override
    public void shutdown() {
        // Called during server shutdown
        // Clean up resources, save data
        getLogger().info("MyPlugin shutting down...");
    }
}
```

### Lifecycle Order

```
Server Boot → setup() → start() → [Running] → shutdown() → Server Stop
```

---

## Command Registration

Register custom commands in `setup()`:

```java
@Override
public void setup() {
    registerCommand("hello", (sender, args) -> {
        sender.sendMessage("Hello from MyPlugin!");
        return true;
    });
    
    registerCommand("spawn", (sender, args) -> {
        if (sender instanceof Player player) {
            player.teleportToSpawn();
            player.sendMessage("Teleported to spawn!");
        }
        return true;
    });
}
```

### Command with Arguments

```java
registerCommand("give", (sender, args) -> {
    if (args.length < 2) {
        sender.sendMessage("Usage: /give <player> <item>");
        return false;
    }
    String playerName = args[0];
    String itemId = args[1];
    // Implementation...
    return true;
});
```

---

## Event System

### Event Types

| Category | Examples |
|----------|----------|
| Server Lifecycle | `BootEvent`, `ShutdownEvent`, `PluginSetupEvent` |
| Player Events | `PlayerJoinEvent`, `PlayerQuitEvent`, `PlayerChatEvent` |
| World Events | `WorldLoadEvent`, `ChunkLoadEvent` |
| Entity Events | `EntitySpawnEvent`, `EntityDamageEvent` |
| Block Events | `BlockBreakEvent`, `BlockPlaceEvent` |

### Registering Event Listeners

```java
@Override
public void setup() {
    registerEventListener(PlayerJoinEvent.class, event -> {
        Player player = event.getPlayer();
        getServer().broadcastMessage(player.getName() + " joined!");
    });
    
    registerEventListener(BlockBreakEvent.class, event -> {
        if (event.getBlock().getType().equals("diamond_ore")) {
            event.getPlayer().sendMessage("You found diamonds!");
        }
    });
}
```

### Cancellable Events

```java
registerEventListener(BlockBreakEvent.class, event -> {
    if (isProtectedArea(event.getBlock().getPosition())) {
        event.setCancelled(true);
        event.getPlayer().sendMessage("This area is protected!");
    }
});
```

---

## Entity Component System (ECS)

Hytale uses ECS architecture for entities:

### Core Concepts

- **Entity**: Unique ID, no data or behavior
- **Component**: Data container (health, position, inventory)
- **System**: Logic that operates on components

### Working with Components

```java
// Get a component from an entity
HealthComponent health = entity.getComponent(HealthComponent.class);
if (health != null) {
    health.setHealth(health.getHealth() + 10);
}

// Check if entity has component
if (entity.hasComponent(FlyingComponent.class)) {
    // Handle flying entity
}
```

---

## Gradle Build Configuration

### build.gradle.kts

```kotlin
plugins {
    java
    id("com.hytale.plugin") version "1.0.0"
}

group = "com.yourname"
version = "1.0.0"

java {
    toolchain {
        languageVersion.set(JavaLanguageVersion.of(25))
    }
}

repositories {
    mavenCentral()
    maven("https://maven.hytale.com/releases")
}

dependencies {
    compileOnly("com.hytale:server-api:1.0.0")
}

hytalePlugin {
    pluginId = "myplugin"
    pluginName = "My Plugin"
    author = "YourName"
    version = project.version.toString()
}
```

### Building

```bash
# Build the plugin JAR
gradle build

# Output: build/libs/MyPlugin-1.0.0.jar
```

### Installing

Copy JAR to: `%APPDATA%/Hytale/UserData/Mods/`

---

## Project Structure

```
MyPlugin/
├── build.gradle.kts          # Gradle build script
├── settings.gradle.kts       # Gradle settings
├── gradle.properties         # Gradle properties
├── src/
│   └── main/
│       ├── java/
│       │   └── com/yourname/myplugin/
│       │       ├── MyPlugin.java        # Main plugin class
│       │       ├── commands/            # Command handlers
│       │       ├── events/              # Event listeners
│       │       └── systems/             # ECS systems
│       └── resources/
│           └── plugin.json              # Plugin metadata
└── build/
    └── libs/
        └── MyPlugin-1.0.0.jar           # Built plugin
```

---

## Quick Reference

| Task | How |
|------|-----|
| Build plugin | `gradle build` |
| Install plugin | Copy JAR to `%APPDATA%/Hytale/UserData/Mods/` |
| Reload plugins | `/plugin reload` in-game |
| View plugins | `/plugin list` in-game |
| Debug | Use Antigravity terminal + logging |

---

## Resources

- **Plugin Template**: [Darkhax-Hytale/HytalePluginTemplate](https://github.com/Darkhax-Hytale/HytalePluginTemplate)
- **Hytale Modding Bible**: Community-curated API reference (Reddit)
- **Video Tutorials**: Kaupenjoe on YouTube
- **Java 25 Reference**: See `java-25-hytale` skill
- **Gradle Reference**: See `gradle-hytale` skill
