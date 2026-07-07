# Changelog

All notable changes to the Conversational Telegram Bot (Katelyn) will be documented here.

---

<img src="media/v2.1.0 Update banner.jpg" alt="Katelyn v2.0 Banner" width="450"/>

# v2.1.0 - Model Optimization, Voice Support & Architecture Improvements

## Added

### AI Features

* Added Text-to-Speech (TTS) and Speech-to-Text (STT) support using `gemini-3.1-flash-tts-preview`
* Added owner override command for role switching
* Split image processing into a dedicated request queue for improved multimodal handling

### Internal Architecture

* Refactored AI response pipeline for improved modularity and maintainability
* Improved separation between image and text request processing

---

## Changed

### AI Models

* Refactored model routing:

  * Private chats → `gemini-3.1-flash-lite`
  * Image processing → `gemini-3.1-flash-lite`
  * Group chats → `gemini-2.5-flash-lite`

### Personality System

* Updated default persona:

  * Introvert → Ambivert
* Refined personality instructions for more balanced conversational behavior

### Infrastructure

* Fully synchronized activity scheduling with the bot's detected location
* Reduced `bot_health` database logging frequency:

  * 6 records → 1 record
* Cleaned up group event handling logic

---

## Fixed

### Stability
* Added seamless fallback experience during text generation errors
* Fixed activity synchronization inconsistencies
* Multiple bug fixes across the request pipeline and internal services

---


<img src="media/V2.0.0 Banner.jpg" alt="Katelyn v2.0 Banner" width="450"/>

# v2.0.0 - PostgreSQL Migration, Multimodal Support & Core Architecture Rewrite

## Added

### Database Layer

* Full PostgreSQL migration
* PostgreSQL connection pooling
* Versioned migration system with execution tracking
* Database schema:

  * `users`
  * `groups`
  * `group_members`
  * `messages`
  * `leaderboard`
  * `ai_requests`
  * `bot_health`
  * `daily_usage`
* Database-driven role management

### Models

* `userModel`

  * createOrGet
  * getById
  * ban
  * unban
  * updateRole
  * count
  * deleteInactive
  * getUsers

* `messagesModel`

  * saveMessage
  * getHistory
  * transactional writes
  * automatic history trimming

* `groupModel`

  * saveGroup
  * deleteGroup
  * updateInteractionTime
  * countGroups
  * getGroups

* `groupMembersModel`

  * addMember
  * removeMember
  * getMemberList

* `aiRequestsModel`

  * saveRequests
  * getRequests
  * request retention controls

* `leaderboardModel`

  * addUser
  * updateScore
  * getScore
  * getLeaderboard
  * upsertUserScore

* `botHealthModel`

  * saveHealth
  * getHealth

* `dailyUsageModel`

  * saveUsage
  * getCount
  * getUsage
  * resetDaily

### Commands

* `/daily`
* `/rank`
* `/quiz`
* `/slot`
* `/truthordare`
* `/ban`
* `/unban`
* `/notify`
* `/ping`
* `/help`
* `/leave`

### Economy & Gamification

* Daily rewards
* Persistent score tracking
* Leaderboard system
* Quiz rewards
* Slot machine rewards
* Rock-Paper-Scissors betting

### Moderation & Administration

* Owner/Admin/User role system
* User ban system
* Broadcast notification system
* Group management tools
* Advanced diagnostics panel

### Multimodal Support

* Image processing support
* Gemini structured JSON outputs
* Dedicated image schema configuration
* Automatic image description extraction
* Image metadata logging

### Scheduling & Automation

* Node-cron integration
* Midnight maintenance tasks
* Daily reward reset system
* Automatic inactive-user cleanup
* Holiday scheduling utility
* Break-aware activity scheduling
* Downtime recovery broadcasts

### Reliability & Infrastructure

* Blind round-robin API key rotation
* Invalid-key blacklist cache
* Automatic key recovery after cooldown
* API quota protection
* Self-healing downtime detection
* Media download timeout protection
* Enhanced error protection
* Bot awareness of current date and time
* Message interval tracking using `deltaSeconds`

### Utilities

* `formatHistory`
* `keyRotation`
* `invalidKeys`
* `isOnBreak`
* Improved image buffer utility
* Diagnostics helpers

---

## Changed

### Database Architecture

* Fully migrated from MongoDB to PostgreSQL
* Reworked persistence layer around SQL transactions
* Improved data integrity through constraints and composite keys
* Database is now the source of truth for permissions and roles

### AI Pipeline

* Migrated to latest Gemini SDK
* Updated generation model:

  * `gemini-2.5-flash-lite` → `gemini-2.5-flash`
* Refactored AI services for PostgreSQL-backed memory
* Unified request tracking
* Improved history formatting
* Added structured multimodal output handling

### Key Management

* Removed live API validation workflow
* Implemented blind round-robin key selection
* Added temporary invalid-key quarantine
* Added automatic key recovery after cooldown
* Improved quota exhaustion handling

### Personality System

* Fully rewritten instruction architecture
* Separate owner and admin configurations
* Improved familiarity scaling
* Improved behavioral consistency
* Enhanced role-aware interactions
* Updated personality and conversational style

### Group Chat Handling

* Added bot short-name support
* Improved reply-to-bot detection
* Added group awareness:

  * roles
  * members
  * metadata
* Improved typing simulation for group chats

### Infrastructure

* Improved Express monitoring services
* Reduced retry interval:

  * 15 minutes → 10 minutes
* Added downtime recovery notifications
* Improved uptime tracking
* Improved activity scheduling

### Command Experience

* Refactored new and existing commands
* Improved UX messaging
* Expanded inline keyboard interactions
* Added detailed internal documentation

---

## Fixed

### AI & API

* Fixed API rotation using only one key
* Fixed Gemini quota recovery edge cases
* Fixed invalid date formatting
* Fixed `randomKey` scope issue in `groupAi.js`
* Fixed missing `userId` parameter in image lookups
* Fixed structured output validation failures
* Fixed Gemini `INVALID_ARGUMENT` multimodal crashes

### Messaging

* Fixed reply-to-bot detection logic
* Fixed image routing issues
* Fixed media messages without captions being ignored
* Fixed group activation behavior
* Fixed dynamic typing support in group chats

### Diagnostics

* Fixed `/ping` Telegram markdown parsing crashes
* Sanitized database exception output
* Fixed uptime reporting inconsistencies
* Fixed startup timestamp corruption

### Scheduling

* Fixed activity scheduling inconsistencies
* Fixed holiday override logic
* Fixed work-hour versus idle-hour timing behavior

### Networking

* Fixed Telegram media download timeout failures
* Added explicit connection timeout protection
* Improved network resilience during media retrieval

### Economy

* Fixed slot-machine race condition
* Enforced atomic balance deductions
* Improved reward consistency

### Stability

* Improved transaction safety
* Improved queue reliability
* Improved failure recovery
* Reduced likelihood of request deadlocks
* Reduced cascading service failures

---

## Removed

### Legacy Infrastructure

* Removed MongoDB dependency
* Removed MongoDB models and related utilities
* Removed `ValidateApiKey`
* Removed owner environment variable dependency
* Removed redundant deployment tracking table
* Removed deprecated SDK remnants
* Removed obsolete API validation workflow

### Deferred Features

* Deferred image generation support
* Deferred prompt/response caching implementation

---

## Migration Notes

### Breaking Changes

* MongoDB is no longer supported
* PostgreSQL is now required
* User role management is database-driven
* Legacy API validation flow has been removed

---

# v1.1.0 - API Resilience & Architecture Refactor

## Added

### API Reliability Layer

* Multi-key Gemini API rotation system
* Initial implementation of API health-check validation before request execution
* Response gating system to reject invalid AI outputs before pipeline continuation.

### Internal Architecture

* Unified AI response generation flow for private and group handling
* Cleaner service separation between:

  * Key selection
  * Key validation
  * Response generation
* Improved modularization of AI-related services
* Added retry handling logic for failed AI response generation
* Added short-name trigger support for easier bot invocation in groups

---

## Changed

### System Refactor

* Refactored command execution flow for cleaner handler structure
* Improved separation of concerns across:

  * Controllers
  * Services
  * Models
  * Utilities
* Increased memory length:

  * 10 → 20

### Personality System

* Updated personality instruction rules for:

  * Better conversational consistency
  * Stronger behavior enforcement
  * Reduced assistant-like responses

---

## Removed

* Redundant npm packages unused in production
* Duplicate utility logic related to queue/request handling
* Legacy OpenAI-specific implementation remnants

---

## Fixed

* Reduced risk of API failures propagating into generation pipeline
* Improved handling during API quota exhaustion
* Improved stability under sustained request load
* Reduced inconsistent response behavior across chat types
* Fixed reply-to-bot detection by explicitly tracking bot username
* Fixed self-ping reliability issues affecting uptime stability

---

# v1.0.0 - Initial Stable Architecture

## Added

### Core System

* Node.js Telegram chatbot core with personality-driven architecture
* Google Gemini API integration for conversational AI responses
* Modular project structure with separated controllers, services, commands, models, and utilities
* Dynamic command auto-loader from `/cmd` directory

### AI & Conversation Engine

* Structured `systemInstruction` personality control system
* Separate AI pipelines for:

  * Private chats (persistent memory)
  * Group chats (stateless contextual mode)
* Reply-context injection system for group conversations
* Human-like conversational pacing through delayed responses
* Typing indicator simulation using Telegram chat actions

### Memory System

* MongoDB-backed persistent memory system
* Automatic user creation and retrieval flow
* Rolling conversation context storage
* Context capped to latest 10 messages per user

### Queue Architecture

* Centralized queue-based request processing system
* Sequential execution flow to prevent concurrent AI calls
* Retry handling system for failed responses
* Delayed retry cooldown system (15-minute requeue)
* Priority queue support for bot owner requests

### Group Chat Support (Beta)

* Mention-based activation
* Name-trigger activation
* Reply-based activation
* Stateless group mode to avoid context pollution
* Context isolation between group users

### Interaction Layer

* Media filtering system for:

  * Stickers
  * Images
  * Videos
  * Voice notes

* Command system:

  * `/start`
  * `/about`
  * `/support`
  * `/callad`

### Infrastructure

* Express.js health-check server
* Render-compatible self-pinging uptime system
* Environment-based configuration using dotenv
* Telegram polling error logging

---

## Changed

### AI Backend Migration

* Migrated from OpenAI Responses API to Google Gemini
* Replaced traditional prompt handling with `systemInstruction`
* Reworked conversational flow into separate private/group execution pipelines

### Queue Improvements

* Reduced race conditions
* Reduced API overload
* Improved request ordering
* Added controlled execution cooldowns

### Conversation Handling

* Improved personality consistency
* Improved context formatting
* Improved reply awareness in group conversations

---

## Fixed

* Reduced risk of overlapping AI executions
* Reduced instability during high message load
* Improved fallback handling for failed AI responses
* Improved message ordering consistency

---

# Notes

* Versions below `v1.0.0` are considered experimental.
* Project now runs exclusively on PostgreSQL.
* Future releases follow semantic versioning:

  * PATCH → bug fixes
  * MINOR → features and improvements
  * MAJOR → architecture rewrites or breaking changes
