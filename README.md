<a name="readme-top"></a>

<!-- PROJECT SHIELDS -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![GNU GPL License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

# 🔹 Jira OmniFocus Sync Plugin

> **A powerful OmniFocus plugin that seamlessly synchronizes your Jira tickets with OmniFocus tasks, keeping your project management workflow unified and up-to-date.**

## 📋 Table of Contents

- [About The Project](#about-the-project)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [How To Run](#how-to-run)
- [Configuration](#configuration)
- [How It Works](#how-it-works)
- [Usage Examples](#usage-examples)
- [Troubleshooting](#troubleshooting)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## 🎯 About The Project

This OmniFocus plugin provides **one-way synchronization** from Jira to OmniFocus, automatically managing your tasks based on your Jira tickets. Whether you're a software developer, project manager, or team lead, this plugin eliminates the manual work of keeping two systems in sync.

### Key Benefits

- ✅ **Automatic Sync**: Keep OmniFocus tasks synchronized with your Jira tickets
- 🔄 **Smart Updates**: Automatically creates, reopens, and completes tasks based on Jira status
- 🏷️ **Flexible Tagging**: Organize tasks with customizable tags and nested tag structures
- 📊 **Real-time Feedback**: Get instant summary of changes (created, reopened, completed tasks)
- 🔒 **Secure**: Credentials stored securely in OmniFocus keychain
- 📱 **Cross-Platform**: Works on macOS, iOS, and iPadOS

## ✨ Features

### Core Functionality

- **🎫 Jira Integration**: Connects directly to your Jira instance using API v3
- **🔍 Custom JQL Queries**: Filter which tickets to sync using any valid JQL query
- **⚡ Incremental Sync**: Only fetches tickets updated since last sync (83-85% faster!)
- **🆕 Task Creation**: Automatically creates new OmniFocus tasks for unmatched Jira tickets
- **♻️ Task Reopening**: Reopens completed tasks if their Jira tickets become active again
- **✅ Task Completion**: Marks tasks as complete when they no longer appear in Jira
- **🏷️ Tag Management**: Supports nested tags (e.g., "Work : Jira") for better organization
- **📝 Rich Task Notes**: Includes Jira ticket URL and description in task notes
- **📈 Sync Statistics**: Shows summary of created, reopened, and completed tasks

### Technical Features

- **No Dependencies**: Standalone plugin using native OmniFocus Plug-In API
- **Pagination Support**: Handles large numbers of Jira tickets efficiently (25 per page)
- **Smart Task Matching**: Identifies tasks by Jira ticket key (e.g., "PROJ-123")
- **Credential Management**: Secure storage with easy reset option (hold Control key)
- **Smart Sync Detection**: Automatically determines when full sync is needed vs incremental
- **Error Handling**: Graceful error messages with validation feedback

## 🔧 Prerequisites

Before installing the plugin, ensure you have:

1. **OmniFocus 3 or later** installed on macOS, iOS, or iPadOS
2. **Jira Account** with API access
3. **Jira API Token** (create one at: https://id.atlassian.com/manage-profile/security/api-tokens)
4. **Internet Connection** for syncing with Jira

## 📥 Installation

### Method 1: Direct Installation (Recommended)

1. **Download the plugin file**:
   ```bash
   curl -O https://raw.githubusercontent.com/PowerSchill/jira-omnifocus/main/jira.omnifocusjs
   ```

2. **Install on macOS**:
   - Double-click `jira.omnifocusjs` to install, OR
   - Move to: `~/Library/Mobile Documents/iCloud~com~omnigroup~OmniFocus/Documents/Plug-Ins/`

3. **Install on iOS/iPadOS**:
   - Transfer `jira.omnifocusjs` via AirDrop or Files app
   - Open in OmniFocus to install

### Method 2: Clone Repository

```bash
# Clone the repository
git clone https://github.com/PowerSchill/jira-omnifocus.git

# Navigate to the directory
cd jira-omnifocus

# Copy to OmniFocus plugin directory (macOS)
cp jira.omnifocusjs ~/Library/Mobile\ Documents/iCloud~com~omnigroup~OmniFocus/Documents/Plug-Ins/
```

### Verification

After installation, verify the plugin appears in OmniFocus:
- **macOS**: `Automation` → `Plug-Ins` → Look for "🔹 Jira: Sync"
- **iOS/iPadOS**: Settings → Automation → Plug-Ins

## 🚀 How To Run

### First Time Setup

1. **Launch the plugin** from OmniFocus:
   - **macOS**: Press `⌘⌃Space` and type "Jira: Sync", OR `Automation` menu → `🔹 Jira: Sync`
   - **iOS/iPadOS**: Automation → Plug-Ins → Jira: Sync

2. **Enter your Jira credentials** when prompted:
   - **User ID**: Your Jira email address (e.g., `user@company.com`)
   - **API Key**: Your Jira API token (not your password!)

3. **Configure sync settings**:
   - **Jira URL**: Your Jira instance (e.g., `https://yourcompany.atlassian.net`)
   - **OmniFocus Tag**: Tag to apply to synced tasks (default: `jira`)
   - **JQL Query**: Filter which tickets to sync (default: `assignee=currentuser() and resolution is empty`)

4. **Click "Continue"** to perform your first sync

### Subsequent Syncs

Simply run the plugin again using the same method. Your credentials and preferences are saved.

### Resetting Credentials

To clear stored credentials:
1. Hold the **Control** key while launching the plugin
2. Click "Reset" when prompted

## ⚙️ Configuration

### OmniFocus Tag

The tag determines which tasks are managed by the plugin:
- **Simple tag**: `jira` (creates/uses top-level tag)
- **Nested tag**: `Work : Jira` (creates/uses nested structure)

### JQL Query Examples

Customize which Jira tickets to sync:

```jql
# All your open tickets (default)
assignee=currentuser() and resolution is empty

# Specific project
project = MYPROJECT and assignee=currentuser() and resolution is empty

# Multiple projects
project in (PROJ1, PROJ2) and assignee=currentuser() and resolution is empty

# By status
assignee=currentuser() and status in ("In Progress", "To Do")

# Exclude certain types
assignee=currentuser() and resolution is empty and type != Epic

# Due soon
assignee=currentuser() and resolution is empty and due <= 7d
```

## 🔄 How It Works

### Sync Process

1. **Authentication**: Validates your Jira credentials
2. **Sync Type Detection**: Determines whether to perform incremental or full sync
3. **Fetch Jira Tickets**: Queries Jira using your JQL with pagination support
4. **Load OmniFocus Tasks**: Gets all tasks with your specified tag
5. **Match & Update**:
   - **New Jira ticket** → Creates new OmniFocus task
   - **Existing task, active ticket** → Reopens task if completed
   - **Existing task, no ticket** → Marks task as complete (if no incomplete subtasks)
6. **Summary**: Displays sync statistics and sync type

### ⚡ Incremental Sync

The plugin automatically uses **incremental sync** to dramatically improve performance:

#### How It Works
- **First sync**: Fetches all tickets (full sync)
- **Subsequent syncs**: Only fetches tickets updated since last sync (incremental)
- **JQL query changed**: Automatically switches to full sync
- **Force full sync**: Hold **Option** key when running the plugin

#### Performance Benefits
- **Full sync**: 150 tickets = ~15-20 seconds
- **Incremental sync**: 5 changed tickets = ~2-3 seconds
- **Speed improvement**: 83-85% faster for typical workloads!

#### When Full Sync Occurs
1. **First time**: No previous sync timestamp exists
2. **Query changed**: Your JQL query was modified since last sync
3. **Manual override**: Hold **Option** key when launching plugin
4. **Safety**: Ensures data consistency when configuration changes

#### Sync Type Indicators
The plugin clearly shows which type of sync was performed:
- **Console log**: "Incremental sync completed" or "Full sync completed"
- **Summary alert**: Shows "🔄 Sync Type: Incremental ⚡" or "Full 🔍"

### Task Naming Convention

**⚠️ Important**: Tasks are identified by their Jira ticket key at the start of the name:

✅ **Valid**:
- `PROJ-123 Fix login bug`
- `ABC-456 Add new feature`
- `DEMO-789 Update documentation`

❌ **Invalid**:
- `Fix login bug PROJ-123` (key not at start)
- `Fix PROJ-123 login bug` (key in middle)

You can edit anything after the ticket key, but **do not change or remove the key itself**.

### Task Properties

Each synced task includes:
- **Name**: `[TICKET-KEY] [Summary from Jira]`
- **Tag**: Your configured sync tag (e.g., `jira`)
- **Note**:
  ```
  [Jira ticket URL]

  [Ticket description from Jira]
  ```
- **Location**: Added to OmniFocus Inbox

## 📖 Usage Examples

### Example 1: Daily Sync

Run the plugin each morning to sync overnight changes:
1. Open OmniFocus
2. Press `⌘⌃Space` → "Jira: Sync"
3. Review the summary of changes

### Example 2: Project-Specific Sync

Sync only tickets from a specific project:
- **JQL**: `project = MYAPP and assignee=currentuser() and resolution is empty`
- **Tag**: `Projects : MyApp`

### Example 3: Force Full Sync

When you need to resync all tickets (e.g., after bulk Jira changes):
1. Hold the **Option** key while launching the plugin
2. The plugin will perform a full sync instead of incremental
3. Useful for recovery or after bulk updates in Jira

### Example 4: Multiple Jira Instances

For different Jira instances, create separate copies of the plugin with different identifiers (advanced users).

## 🐛 Troubleshooting

### Common Issues

| Issue                          | Solution                                                                                 |
| ------------------------------ | ---------------------------------------------------------------------------------------- |
| **"API Key is invalid"**       | Regenerate your API token at https://id.atlassian.com/manage-profile/security/api-tokens |
| **No tasks created**           | Verify your JQL query returns results in Jira's Issue Navigator                          |
| **Tasks not reopening**        | Check that ticket key is at the start of the task name                                   |
| **"Could not create tag"**     | Ensure tag name doesn't contain special characters (except `:`)                          |
| **Slow sync**                  | Plugin now uses incremental sync automatically! Hold Option for full sync if needed      |
| **Missing recent tickets**     | Try a full sync by holding Option key - may happen after bulk Jira updates               |
| **Want to force full sync**    | Hold **Option** key when launching the plugin                                            |

### Debug Mode

Console logging is enabled by default. View logs:
- **macOS**: Console.app → Filter for "OmniFocus"
- Check for detailed sync information and error messages

### Getting Help

1. Check [open issues](https://github.com/PowerSchill/jira-omnifocus/issues)
2. Review [troubleshooting guide](https://github.com/PowerSchill/jira-omnifocus/issues?q=label%3A%22help+wanted%22)
3. Open a new issue with:
   - Console log output
   - Your JQL query (sanitized)
   - OmniFocus version
   - macOS/iOS version

## 🗺️ Roadmap

See the [open issues](https://github.com/PowerSchill/jira-omnifocus/issues) for a full list of proposed features and known issues.

### High Priority
- [x] Basic Jira sync functionality
- [x] Sync statistics and feedback
- [x] Incremental sync (only fetch updated tickets) - [#21](https://github.com/PowerSchill/jira-omnifocus/issues/21)
- [ ] Custom field mapping (priority, due dates) - [#20](https://github.com/PowerSchill/jira-omnifocus/issues/20)

### Future Enhancements
- [ ] Bidirectional sync (update Jira from OmniFocus) - [#32](https://github.com/PowerSchill/jira-omnifocus/issues/32)
- [ ] Sync Jira comments to task notes - [#26](https://github.com/PowerSchill/jira-omnifocus/issues/26)
- [ ] Multiple Jira instance support - [#30](https://github.com/PowerSchill/jira-omnifocus/issues/30)
- [ ] Project-based auto-tagging - [#23](https://github.com/PowerSchill/jira-omnifocus/issues/23)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
## License

Distributed under the GNU GENERAL PUBLIC LICENSE License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->
## Contact

Project Link: [https://github.com/PowerSchill/jira-omnifocus](https://github.com/PowerSchill/jira-omnifocus)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

- [Bastian-Kuhn/jira-omnifocus](https://github.com/Bastian-Kuhn/jira-omnifocus) for doing the hard work and getting me started.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/PowerSchill/jira-omnifocus.svg?style=for-the-badge
[contributors-url]: https://github.com/PowerSchill/jira-omnifocus/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/PowerSchill/jira-omnifocus.svg?style=for-the-badge
[forks-url]: https://github.com/PowerSchill/jira-omnifocus/network/members
[stars-shield]: https://img.shields.io/github/stars/PowerSchill/jira-omnifocus.svg?style=for-the-badge
[stars-url]: https://github.com/PowerSchill/jira-omnifocus/stargazers
[issues-shield]: https://img.shields.io/github/issues/PowerSchill/jira-omnifocus.svg?style=for-the-badge
[issues-url]: https://github.com/PowerSchill/jira-omnifocus/issues
[license-shield]: https://img.shields.io/github/license/PowerSchill/jira-omnifocus.svg?style=for-the-badge
[license-url]: https://github.com/PowerSchill/jira-omnifocus/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/markschill
