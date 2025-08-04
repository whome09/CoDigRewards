# CoDigReward Plugin Wiki


------

## 🎯 Introduction

**CoDigReward** is a powerful Minecraft Bukkit/Spigot plugin that allows server administrators to set reward tasks for players breaking specific blocks. The plugin supports one-time tasks, repeatable tasks, time-limited tasks, and various other types.

### Main Features

- ✅ Support for reward tasks for any block type
- ✅ One-time and repeatable tasks
- ✅ Time-limited tasks (start/end time)
- ✅ Multi-language support (Chinese, English, French)
- ✅ Custom reward messages
- ✅ Player progress tracking
- ✅ Asynchronous data saving
- ✅ Complete permission system
- ✅ Tab completion functionality

------

## 🔧 Installation & Configuration

### System Requirements

- Minecraft Version: 1.21+
- Server: Bukkit/Spigot/Paper
- Java Version: 17+

### Installation Steps

1. Download the `CoDigReward.jar` file
2. Place the file in the server's `plugins` folder
3. Restart the server
4. The plugin will automatically generate configuration files and language files

### File Structure

```
plugins/CoDigReward/
├── config.yml          # Main configuration file
├── player_data.yml      # Player data file
└── lang/                # Language file folder
    ├── en.yml          # English language file
    ├── fr.yml          # French language file
    └── zh.yml          # Chinese language file
```

------

## 💻 Command System

### Main Commands

- **Base Command**: `/reward` or `/codigreward` or `/cr` or `/rewards`
- **Permission Required**: `blockreward.reward`

### Subcommand Details

#### `/reward add <task_name> <block_type> <amount> <reward_item> <reward_amount> <one_time> [start_time] [end_time]`

Add a new reward task

**Parameter Description:**

- `task_name`: Unique identifier for the task
- `block_type`: Target block type (e.g., STONE, OAK_LOG)
- `amount`: Number of blocks to be broken
- `reward_item`: Type of reward item (e.g., DIAMOND, IRON_INGOT)
- `reward_amount`: Amount of reward items
- `one_time`: true (one-time) or false (repeatable)
- `start_time`: Task start time (optional, format: yyyy-MM-dd HH:mm:ss)
- `end_time`: Task end time (optional, format: yyyy-MM-dd HH:mm:ss)

**Examples:**

```
/reward add stone_task STONE 50 DIAMOND 1 false none none
/reward add special_event COAL_ORE 100 GOLDEN_APPLE 1 true "2025-08-01 00:00:00" "2025-08-31 23:59:59"
```

#### `/reward remove <task_name>`

Remove the specified task

#### `/reward list`

Display all configured tasks and their status

#### `/reward info <task_name>`

Display detailed information about the specified task

#### `/reward progress [player_name]`

Display task progress (player name is optional, requires additional permission to view others' progress)

#### `/reward reload`

Reload plugin configuration

------

## 🔐 Permission System

### Basic Permissions

- `blockreward.reward`: Permission to use basic commands (default: OP)
- `blockreward.receive`: Permission to receive rewards (default: everyone)

### Admin Permissions

- `blockreward.admin`: Complete administrator permissions (default: OP)
- `blockreward.add`: Permission to add tasks (default: OP)
- `blockreward.remove`: Permission to remove tasks (default: OP)
- `blockreward.reload`: Permission to reload configuration (default: OP)

### View Permissions

- `blockreward.list`: Permission to view task list (default: everyone)
- `blockreward.info`: Permission to view task information (default: everyone)
- `blockreward.progress.self`: Permission to view own progress (default: everyone)
- `blockreward.progress.others`: Permission to view others' progress (default: OP)

### Special Permissions

- `blockreward.bypass`: Bypass task restrictions (such as time limits)

------

## ⚙️ Configuration File

### config.yml Configuration Description

```yaml
# Language setting (en, fr, zh)
language: zh

# Data saving settings
data:
  # Auto-save interval (minutes)
  auto_save_interval: 5
  # Whether to save data when player goes offline
  save_on_quit: true

# Performance settings
performance:
  # Whether to enable asynchronous data saving
  async_save: true
  # Whether to enable language file caching
  cache_language_files: true

# Reward task configuration
rewards:
  # Configure your tasks here...
```

### Configuration Options Details

| Configuration Item                 | Default | Description                            |
| ---------------------------------- | ------- | -------------------------------------- |
| `language`                         | `zh`    | Plugin language (en/fr/zh)             |
| `data.auto_save_interval`          | `5`     | Auto-save interval (minutes)           |
| `data.save_on_quit`                | `true`  | Whether to save data when player quits |
| `performance.async_save`           | `true`  | Whether to enable asynchronous saving  |
| `performance.cache_language_files` | `true`  | Whether to cache language files        |

------

## 🎯 Task Configuration

### Task Configuration Format

```yaml
rewards:
  task_name:
    block: block_type              # e.g., STONE, OAK_LOG
    amount: required_amount        # integer
    reward: reward_item            # e.g., DIAMOND, IRON_INGOT  
    rewardAmount: reward_amount    # integer
    oneTime: is_one_time          # true/false
    startTime: start_time         # yyyy-MM-dd HH:mm:ss or leave empty
    endTime: end_time             # yyyy-MM-dd HH:mm:ss or leave empty
    message: custom_message       # can be empty to use default message
```

### Task Type Examples

#### 1. Basic Repeatable Task

```yaml
stone_mining:
  block: STONE
  amount: 50
  reward: IRON_INGOT
  rewardAmount: 5
  oneTime: false
  startTime: ""
  endTime: ""
  message: "§eYou received {amount} iron ingots!"
```

#### 2. One-time Task

```yaml
first_diamond:
  block: DIAMOND_ORE
  amount: 1
  reward: DIAMOND_PICKAXE
  rewardAmount: 1
  oneTime: true
  startTime: ""
  endTime: ""
  message: "§bCongratulations! You received a diamond pickaxe as first mining reward!"
```

#### 3. Time-limited Event Task

```yaml
summer_event:
  block: COAL_ORE
  amount: 200
  reward: GOLDEN_APPLE
  rewardAmount: 3
  oneTime: true
  startTime: "2025-07-01 00:00:00"
  endTime: "2025-08-31 23:59:59"
  message: "§6Summer event reward! You received golden apples!"
```

### Message Placeholders

The following placeholders can be used in custom messages:

- `{amount}`: Reward amount
- `{item}`: Reward item name
- `{required}`: Number of blocks to be broken
- `{block}`: Block type name

------

## 🌍 Language Support

### Supported Languages

- **Chinese (zh)**: Simplified Chinese
- **English (en)**: English
- **French (fr)**: Français

### Adding Custom Languages

1. Create a new language file in the `lang` folder (e.g., `de.yml`)
2. Copy the content from an existing language file and translate
3. Set `language: de` in `config.yml`
4. Reload the plugin

### Custom Message Keys

```yaml
you_received_reward: "You received reward message"
task_completed: "Task completed message"
task_progress: "Task progress message"
no_permission: "No permission message"
# ... more message keys
```

------

## ❓ FAQ

### Q: What to do if the plugin doesn't work?

**A:** Check the following:

1. Confirm server version compatibility (1.21+)
2. Check console for error messages
3. Confirm configuration file format is correct
4. Restart the server

### Q: What to do if tasks don't trigger?

**A:** Check:

1. Whether the task is within active time
2. Whether the player has `blockreward.receive` permission
3. Whether the block type is correct (case sensitive)
4. Whether it's a one-time task that has already been completed

### Q: How to backup player data?

**A:** Regularly backup the `player_data.yml` file.

### Q: What happens to rewards when inventory is full?

**A:** The plugin will automatically drop items at the player's feet.

### Q: Can multiple rewards be set?

**A:** The current version supports only one reward item per task, but multiple tasks can be created.

### Q: How to view all players' progress?

**A:** Use the `/reward progress <player_name>` command (requires admin permission).

------

## 🤝 Support & Feedback

If you encounter problems or have feature suggestions, please:

1. Check the FAQ section of this Wiki
2. Submit an Issue on the GitHub repository
3. Contact the plugin author
