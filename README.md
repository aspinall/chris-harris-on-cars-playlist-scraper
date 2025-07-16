# Chris Harris on Cars Playlist Scraper

A TypeScript application that automatically extracts music recommendations from YouTube podcast episodes and creates playlists on streaming services. Built with Domain-Driven Design principles and hexagonal architecture for maintainability and extensibility.

## 🎵 Features

- **Automatic Transcript Extraction**: Fetches transcripts from YouTube podcast episodes
- **Intelligent Music Parsing**: Uses NLP to identify artist and song mentions from podcast transcripts
- **Multi-Service Support**: Built with extensible architecture (Spotify implemented, Apple Music ready)
- **Smart Playlist Management**: Creates and manages playlists automatically with confidence scoring
- **Weekly Automation**: Scheduled processing of new episodes

## 🏗️ Architecture

This project follows Domain-Driven Design (DDD) principles with hexagonal architecture:

- **Domain Layer**: Core business logic, entities, and repository interfaces
- **Application Layer**: Use cases and application services
- **Infrastructure Layer**: External service adapters (Spotify, YouTube, NLP parsers)
- **Presentation Layer**: CLI interface and schedulers

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ and npm
- Spotify Developer Account with API credentials
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/aspinall/chris-harris-on-cars-playlist-scraper.git
cd chris-harris-on-cars-playlist-scraper

# Install dependencies
npm install

# Copy environment template
cp .env.example .env

# Configure your Spotify credentials in .env
```

### Configuration

Create a `.env` file with your Spotify API credentials:

```env
SPOTIFY_CLIENT_ID=your_spotify_client_id
SPOTIFY_CLIENT_SECRET=your_spotify_client_secret
SPOTIFY_REDIRECT_URI=https://example.com/callback
```

### Basic Usage

```bash
# Process a single episode
npm run process-episode --url="https://youtube.com/watch?v=VIDEO_ID"

# Start weekly automation
npm run start-scheduler

# Run with development mode
npm run dev
```

## 📋 Development Status

This project is currently in development. See [PROJECT_PLAN.md](PROJECT_PLAN.md) for the complete implementation roadmap.

### Current Phase: Foundation & Spotify Integration
- [ ] TypeScript project setup with tooling
- [ ] Spotify OAuth implementation  
- [ ] Core domain entities
- [ ] Configuration management

### Upcoming Phases
- YouTube transcript extraction
- Music mention parsing with NLP
- Application orchestration layer
- CLI interface and automation

## 🔧 Development

### Project Structure

```
src/
├── domain/           # Core business logic
├── infrastructure/   # External service adapters
├── application/      # Use cases and services  
├── presentation/     # CLI and schedulers
├── config/          # Configuration management
└── shared/          # Common utilities
```

### Running Tests

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run integration tests
npm run test:integration
```

### Linting and Formatting

```bash
# Lint code
npm run lint

# Format code
npm run format

# Type checking
npm run type-check
```

## 🎯 Usage Examples

### Processing a Podcast Episode

```typescript
import { EpisodeProcessingService } from './src/application/services/EpisodeProcessingService';

const processor = new EpisodeProcessingService();
const result = await processor.processEpisode({
  youtubeUrl: 'https://youtube.com/watch?v=VIDEO_ID',
  playlistName: 'Chris Harris Music - Episode 123'
});

console.log(`Created playlist with ${result.tracksAdded} tracks`);
```

### Searching for Music

```typescript
import { SpotifyRepository } from './src/infrastructure/repositories/SpotifyRepository';

const spotify = new SpotifyRepository();
const tracks = await spotify.searchTrack('Radiohead', 'Creep');
console.log(`Found ${tracks.length} matching tracks`);
```

## 🔒 Security

- API credentials are stored securely using environment variables
- OAuth tokens are encrypted at rest
- No sensitive data is logged or committed to the repository

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Quick Contributing Steps

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass (`npm test`)
6. Commit your changes (`git commit -m 'Add amazing feature'`)
7. Push to the branch (`git push origin feature/amazing-feature`)
8. Open a Pull Request

## 📚 API Documentation

### Core Entities

- **PodcastEpisode**: Represents a podcast episode with transcript
- **MusicRecommendation**: Parsed music mention with confidence score
- **MusicTrack**: Track information from streaming service
- **Playlist**: Collection of tracks on streaming service

### Repository Interfaces

- **IMusicServiceRepository**: Abstraction for music streaming services
- **IPodcastRepository**: YouTube episode and metadata operations
- **ITranscriptRepository**: Transcript extraction and processing

## 🐛 Troubleshooting

### Common Issues

**"Spotify authentication failed"**
- Verify your client ID and secret in `.env`
- Ensure redirect URI matches your Spotify app settings

**"Transcript not found"**
- Check if the YouTube video has captions/subtitles available
- Verify the video URL is accessible and public

**"No music mentions found"**
- The episode may not contain music recommendations
- Try adjusting the confidence threshold in configuration

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [youtube-transcript](https://www.npmjs.com/package/youtube-transcript) for transcript extraction
- [compromise.js](https://github.com/spencermountain/compromise) for natural language processing
- [Spotify Web API](https://developer.spotify.com/documentation/web-api/) for music service integration

## 📞 Support

- 🐛 [Report bugs](https://github.com/aspinall/chris-harris-on-cars-playlist-scraper/issues)
- 💡 [Request features](https://github.com/aspinall/chris-harris-on-cars-playlist-scraper/issues)
- 📖 [Read the docs](PROJECT_PLAN.md)

---

**Made with ❤️ for podcast music discovery**