# TypeScript Music Recommendation Extractor - Project Plan

## Current Status Analysis
You have tested Spotify API endpoints using cURL commands and have working credentials. The existing files are scratchpad/testing materials, not implemented code. Need to build complete Spotify integration from scratch.

## 1. Directory Structure (DDD/Hexagonal Architecture)

```
src/
├── domain/                           # Core business logic
│   ├── entities/
│   │   ├── PodcastEpisode.ts
│   │   ├── MusicRecommendation.ts
│   │   ├── MusicTrack.ts
│   │   └── Playlist.ts
│   ├── repositories/                 # Port interfaces
│   │   ├── IMusicServiceRepository.ts
│   │   ├── IPodcastRepository.ts
│   │   └── ITranscriptRepository.ts
│   ├── services/                     # Domain services
│   │   ├── MusicMentionParser.ts
│   │   └── PlaylistOrchestrator.ts
│   └── value-objects/
│       ├── TrackId.ts
│       ├── PlaylistId.ts
│       └── Timestamp.ts
├── infrastructure/                   # External adapters
│   ├── repositories/
│   │   ├── SpotifyRepository.ts
│   │   ├── YouTubeRepository.ts
│   │   └── TranscriptRepository.ts
│   ├── external/
│   │   ├── spotify/
│   │   │   ├── SpotifyClient.ts
│   │   │   ├── SpotifyAuth.ts
│   │   │   └── SpotifyTypes.ts
│   │   └── youtube/
│   │       └── YouTubeClient.ts
│   └── parsers/
│       └── MusicMentionNLPParser.ts
├── application/                      # Use cases/application services
│   ├── services/
│   │   ├── EpisodeProcessingService.ts
│   │   └── PlaylistManagementService.ts
│   └── dto/
│       ├── ProcessEpisodeRequest.ts
│       └── CreatePlaylistRequest.ts
├── presentation/                     # CLI/API interfaces
│   ├── cli/
│   │   └── PodcastPlaylistCLI.ts
│   └── schedulers/
│       └── WeeklyScheduler.ts
├── config/
│   ├── Config.ts
│   ├── ServiceConfiguration.ts
│   └── secrets/
│       └── ApiCredentials.ts
└── shared/
    ├── types/
    ├── errors/
    └── utils/
```

## 2. Implementation Order & Dependencies

### Phase 1: Foundation & Spotify Integration (Days 1-3)
1. Setup TypeScript project structure with tooling (ESLint, Prettier, Jest)
2. **Build complete Spotify integration from scratch:**
   - SpotifyAuth.ts: OAuth flow implementation
   - SpotifyClient.ts: API wrapper with rate limiting
   - SpotifyRepository.ts: Domain repository implementation
   - SpotifyTypes.ts: TypeScript interfaces for API responses
3. Implement domain entities and value objects
4. Create configuration management system

### Phase 2: YouTube & Parsing (Days 4-6)
1. Implement YouTube transcript extraction using `youtube-transcript`
2. Build music mention parser using `compromise.js` for NLP
3. Create transcript repository
4. Define repository interfaces

### Phase 3: Application Layer (Days 7-8)
1. Create episode processing service
2. Build playlist management service
3. Implement orchestration logic

### Phase 4: Integration & Testing (Days 9-10)
1. Create CLI interface
2. Implement comprehensive test suite
3. Add error handling and validation

### Phase 5: Automation (Day 11)
1. Build weekly scheduler
2. Add monitoring and logging
3. Create deployment configuration

## 3. Spotify Integration Implementation Details

### SpotifyAuth.ts - OAuth Flow
- Authorization code flow implementation
- Token refresh mechanism
- Secure token storage
- Error handling for auth failures

### SpotifyClient.ts - API Wrapper
- HTTP client with retry logic
- Rate limiting (Spotify: 100 requests/minute)
- Request/response logging
- Error mapping to domain exceptions

### SpotifyRepository.ts - Domain Integration
- Implements IMusicServiceRepository interface
- Maps Spotify responses to domain entities
- Handles search result filtering and ranking
- Playlist creation and management

### Required Spotify API Operations
- **Search**: `GET /v1/search` - Find tracks by artist/title
- **User Info**: `GET /v1/me` - Get current user profile
- **Create Playlist**: `POST /v1/users/{user_id}/playlists`
- **Add Tracks**: `POST /v1/playlists/{playlist_id}/tracks`
- **Get Playlists**: `GET /v1/me/playlists`

## 4. Key Technical Decisions

### Spotify Integration Architecture
- **Authentication**: OAuth 2.0 Authorization Code Flow
- **HTTP Client**: `axios` with interceptors for auth headers
- **Rate Limiting**: Custom rate limiter respecting Spotify limits
- **Error Handling**: Exponential backoff for transient failures

### YouTube Transcript Extraction
- **Library**: `youtube-transcript` (most reliable, TypeScript-friendly)
- **Fallback**: `youtube-transcript-plus` for advanced features
- **Error Handling**: Graceful degradation when transcripts unavailable

### Music Mention Parsing
- **Primary**: `compromise.js` for English text processing
- **Pattern Matching**: Regex patterns for common formats:
  - "Artist - Song"
  - "Song by Artist" 
  - "Artist's Song"
- **Confidence Scoring**: Based on context clues and formatting

## 5. Testing Strategy

### Unit Tests (Jest)
- Spotify client API operations
- OAuth flow components
- Domain entities and value objects
- Parser accuracy with test music mentions

### Integration Tests
- **Spotify API integration** (using test credentials from your cURL tests)
- YouTube transcript extraction (using known video IDs)
- End-to-end episode processing

### Contract Tests
- Repository interface compliance
- External API response handling
- Spotify API response mapping

## 6. Configuration Management

### Environment Configuration
```typescript
interface Config {
  spotify: {
    clientId: string;        // From your existing credentials
    clientSecret: string;    // From your existing credentials
    redirectUri: string;     // https://example.com/callback
    scopes: string[];        // ['playlist-modify-private', 'playlist-read-private']
  };
  youtube: {
    apiKey?: string;         // Optional for enhanced features
  };
  parsing: {
    confidenceThreshold: number;
    maxRecommendationsPerEpisode: number;
  };
}
```

### Secrets Management
- Environment variables for API credentials (use your existing client ID/secret)
- Encrypted config files for deployment
- Development vs production configurations

## 7. Key Files & Responsibilities

### Spotify Integration (New Implementation)
- `SpotifyAuth.ts`: Complete OAuth 2.0 implementation
- `SpotifyClient.ts`: HTTP API wrapper with rate limiting
- `SpotifyRepository.ts`: Domain repository implementing search/playlist operations
- `SpotifyTypes.ts`: TypeScript interfaces for all API responses

### Core Domain Files
- `MusicMentionParser.ts`: Extract artist/song pairs from transcript text
- `PlaylistOrchestrator.ts`: Coordinate episode processing workflow

### Critical Infrastructure
- `YouTubeClient.ts`: Transcript extraction and episode metadata
- `MusicMentionNLPParser.ts`: Advanced parsing using compromise.js
- `EpisodeProcessingService.ts`: Main application orchestration

### Configuration
- `Config.ts`: Centralized configuration with your Spotify credentials
- `ApiCredentials.ts`: Secure credential handling

## 8. Dependencies & Package Installation

### Core Dependencies
```json
{
  "axios": "^1.6.0",           // HTTP client for Spotify API
  "youtube-transcript": "^1.2.1",  // YouTube transcript extraction
  "compromise": "^14.10.0",    // NLP for music mention parsing
  "dotenv": "^16.3.1",         // Environment configuration
  "winston": "^3.11.0"         // Logging
}
```

### Development Dependencies
```json
{
  "typescript": "^5.3.0",
  "jest": "^29.7.0",
  "@types/jest": "^29.5.0",
  "eslint": "^8.55.0",
  "prettier": "^3.1.0"
}
```

## 9. Domain Entities Design

### PodcastEpisode
```typescript
class PodcastEpisode {
  constructor(
    public readonly url: string,
    public readonly title: string,
    public readonly publishDate: Date,
    public readonly transcript?: string
  ) {}
}
```

### MusicRecommendation
```typescript
class MusicRecommendation {
  constructor(
    public readonly artist: string,
    public readonly trackName: string,
    public readonly album?: string,
    public readonly timestamp?: Timestamp,
    public readonly confidence: number = 0.5
  ) {}
}
```

### MusicTrack
```typescript
class MusicTrack {
  constructor(
    public readonly artist: string,
    public readonly title: string,
    public readonly album?: string,
    public readonly serviceId?: string,
    public readonly serviceUri?: string
  ) {}
}
```

### Playlist
```typescript
class Playlist {
  constructor(
    public readonly name: string,
    public readonly description: string,
    public readonly serviceId: string,
    public readonly tracks: MusicTrack[] = []
  ) {}
}
```

## 10. Repository Interfaces

### IMusicServiceRepository
```typescript
interface IMusicServiceRepository {
  searchTrack(artist: string, trackName: string): Promise<MusicTrack[]>;
  createPlaylist(name: string, description: string): Promise<Playlist>;
  addTracksToPlaylist(playlistId: string, tracks: MusicTrack[]): Promise<void>;
  getPlaylists(): Promise<Playlist[]>;
}
```

### IPodcastRepository
```typescript
interface IPodcastRepository {
  getEpisode(url: string): Promise<PodcastEpisode>;
  getLatestEpisode(channelUrl: string): Promise<PodcastEpisode>;
}
```

### ITranscriptRepository
```typescript
interface ITranscriptRepository {
  getTranscript(videoUrl: string): Promise<string>;
}
```

## 11. Music Mention Parser Patterns

### Regex Patterns for Music Mentions
```typescript
const MUSIC_PATTERNS = [
  /(?:track|song)\s*["']([^"']+)["']\s*by\s*["']?([^"'\n]+)["']?/gi,
  /["']([^"']+)["']\s*[-–—]\s*["']?([^"'\n]+)["']?/gi,
  /([A-Z][a-zA-Z\s]+)'s\s*["']([^"']+)["']/gi,
  /artist\s*["']([^"']+)["']\s*song\s*["']([^"']+)["']/gi
];
```

### Confidence Scoring Factors
- Proximity to music-related keywords
- Proper noun capitalization
- Quote usage
- Context indicators ("plays", "listening to", "track")

## 12. Error Handling Strategy

### Custom Error Classes
- `SpotifyApiError`: Spotify API failures
- `TranscriptNotFoundError`: YouTube transcript unavailable
- `ParseError`: Music mention parsing failures
- `ConfigurationError`: Missing or invalid configuration

### Graceful Degradation
- Continue processing if some tracks can't be found
- Log failures for manual review
- Provide partial results rather than complete failure

This plan provides a comprehensive roadmap for building a maintainable, extensible music recommendation extraction system using Domain-Driven Design principles and hexagonal architecture.