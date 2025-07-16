# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a TypeScript application that extracts music recommendations from YouTube podcast episodes and creates playlists on streaming services. The project follows Domain-Driven Design (DDD) principles with hexagonal architecture.

## Key Architecture Concepts

### Domain-Driven Design Structure
- **Domain Layer**: Core business logic with entities (PodcastEpisode, MusicRecommendation, MusicTrack, Playlist) and repository interfaces
- **Application Layer**: Use cases and orchestration services (EpisodeProcessingService, PlaylistManagementService)
- **Infrastructure Layer**: External service adapters for Spotify, YouTube, and NLP parsing
- **Presentation Layer**: CLI interface and schedulers

### Repository Pattern Implementation
The project uses repository interfaces to abstract external services:
- `IMusicServiceRepository`: Spotify integration (searchTrack, createPlaylist, addTracksToPlaylist)
- `IPodcastRepository`: YouTube episode operations
- `ITranscriptRepository`: Transcript extraction and processing

## Development Commands

Currently in development phase. When implemented, the project will use:

```bash
# Development
npm run dev                    # Run in development mode
npm run build                  # Build TypeScript to JavaScript
npm run type-check            # TypeScript type checking

# Testing
npm test                      # Run all tests
npm run test:coverage         # Run tests with coverage
npm run test:integration      # Run integration tests

# Code Quality
npm run lint                  # ESLint checking
npm run format                # Prettier formatting

# Application Usage
npm run process-episode --url="https://youtube.com/watch?v=VIDEO_ID"
npm run start-scheduler       # Weekly automation
```

## Technology Stack

### Core Dependencies
- **axios**: HTTP client for Spotify API integration
- **youtube-transcript**: Extract transcripts from YouTube videos
- **compromise.js**: NLP library for parsing music mentions from text
- **dotenv**: Environment variable management
- **winston**: Logging

### Development Tools
- **TypeScript 5.3+**: Type safety and modern JavaScript features
- **Jest**: Testing framework with coverage reporting
- **ESLint + Prettier**: Code quality and formatting

## Environment Configuration

Required environment variables in `.env`:
```env
SPOTIFY_CLIENT_ID=your_spotify_client_id
SPOTIFY_CLIENT_SECRET=your_spotify_client_secret
SPOTIFY_REDIRECT_URI=https://example.com/callback
```

## Music Mention Parsing Strategy

The system uses regex patterns to identify music mentions in transcripts:
- `"Artist - Song"` format
- `"Song by Artist"` format  
- `"Artist's Song"` format
- Context-based confidence scoring using proximity to music keywords

## Spotify Integration Architecture

### OAuth Flow
- Authorization Code Flow implementation in `SpotifyAuth.ts`
- Token refresh mechanism with secure storage
- Rate limiting (100 requests/minute)

### API Operations
- Search tracks: `GET /v1/search`
- Get user info: `GET /v1/me`
- Create playlists: `POST /v1/users/{user_id}/playlists`
- Add tracks: `POST /v1/playlists/{playlist_id}/tracks`

## Error Handling Strategy

### Custom Error Classes
- `SpotifyApiError`: Spotify API failures with retry logic
- `TranscriptNotFoundError`: YouTube transcript unavailability
- `ParseError`: Music mention parsing failures
- `ConfigurationError`: Missing/invalid configuration

### Graceful Degradation
- Continue processing if some tracks can't be found
- Log failures for manual review rather than complete failure
- Provide partial results with confidence scoring

## Implementation Status

**Current Phase**: Foundation & Spotify Integration (see PROJECT_PLAN.md)
- [ ] TypeScript project setup with tooling
- [ ] Complete Spotify OAuth implementation
- [ ] Core domain entities and value objects
- [ ] Configuration management system

**Next Phases**: YouTube transcript extraction → Music parsing → Application orchestration → CLI interface

## Testing Approach

### Test Strategy
- **Unit Tests**: Domain entities, value objects, parser accuracy
- **Integration Tests**: Spotify API operations, YouTube transcript extraction
- **Contract Tests**: Repository interface compliance

### Test Data
- Use existing Spotify credentials for integration testing
- Known YouTube video IDs for transcript testing
- Sample music mention text for parser validation