# JioCinema - Video Streaming Platform

A modern video streaming platform built with React and Spring Boot, featuring a sleek dark theme and advanced video playback capabilities.

## Features

### Video Playback
- HLS (HTTP Live Streaming) support with adaptive bitrate
- Multiple quality options (Full HD, HD, SD)
- Auto-quality selection based on network conditions
- Fullscreen support
- Custom video controls
- Play/Pause, Mute/Unmute controls
- Progress bar with time display
- Video sharing functionality

### Content Discovery
- "More Like This" recommendations
- Content categorization by genre and language
- Responsive content sliders
- Video thumbnails and previews
- Search functionality
- Content tabs (Episodes, More Like This)

### User Interface
- Modern dark theme design
- Responsive layout for all devices
- Material Design components
- Smooth animations and transitions
- User-friendly navigation
- Loading states and error handling

### Technical Features
- React with hooks for state management
- Material-UI for consistent design
- HLS.js for video streaming
- Spring Boot backend
- JWT authentication
- PostgreSQL database
- RESTful API integration
- Responsive image loading
- Web Share API integration

## Prerequisites

- Node.js (v16 or higher)
- Java Development Kit (JDK) 17 or higher
- PostgreSQL 13 or higher
- Modern web browser with HLS support
- FFmpeg (for video encoding)
- Maven 3.8+ or Gradle 7+

## Project Structure

```
JioCinema/
├── frontend/                # React frontend
│   ├── public/             # Public assets
│   ├── src/
│   │   ├── components/     # Reusable components
│   │   ├── pages/         # Page components
│   │   ├── services/      # API services
│   │   └── assets/        # Static assets
│   └── package.json
├── backend/                # Spring Boot backend
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/
│   │   │   │       └── jiocinema/
│   │   │   │           ├── config/     # Configuration classes
│   │   │   │           ├── controller/ # REST controllers
│   │   │   │           ├── model/      # Entity classes
│   │   │   │           ├── repository/ # Data repositories
│   │   │   │           ├── service/    # Business logic
│   │   │   │           └── security/   # Security config
│   │   │   └── resources/
│   │   │       ├── application.properties
│   │   │       └── application-dev.properties
│   │   └── test/          # Unit tests
│   ├── pom.xml            # Maven dependencies
│   └── README.md
├── assets/                 # Video files and thumbnails
└── README.md              # Project documentation
```

## Getting Started

### Backend Setup

1. Configure PostgreSQL:
   ```sql
   CREATE DATABASE jiocinema;
   CREATE USER jiocinema WITH PASSWORD 'your_password';
   GRANT ALL PRIVILEGES ON DATABASE jiocinema TO jiocinema;
   ```

2. Configure application properties:
   ```properties
   # src/main/resources/application.properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/jiocinema
   spring.datasource.username=jiocinema
   spring.datasource.password=your_password
   spring.jpa.hibernate.ddl-auto=update
   spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
   ```

3. Navigate to backend directory and run:
   ```bash
   # For Maven
   ./mvnw spring-boot:run

   # For Gradle
   ./gradlew bootRun
   ```

### Frontend Setup

1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm start
   ```

4. Open [http://localhost:3000](http://localhost:3000) in your browser

### Video Requirements

- Supported formats: MP4, WebM
- Recommended resolutions:
  - Full HD (1080p)
  - HD (720p)
  - SD (480p)
- HLS stream format (.m3u8)

## API Endpoints

### Authentication
- `POST /api/auth/signup` - Register new user
- `POST /api/auth/login` - User login
- `POST /api/auth/refresh` - Refresh JWT token

### Videos
- `GET /api/videos` - Get all videos
- `GET /api/videos/{id}` - Get video by ID
- `GET /api/videos/search` - Search videos
- `POST /api/videos` - Upload new video (requires auth)
- `PUT /api/videos/{id}` - Update video (requires auth)
- `DELETE /api/videos/{id}` - Delete video (requires auth)

### User
- `GET /api/users/profile` - Get user profile
- `PUT /api/users/profile` - Update user profile
- `GET /api/users/watchlist` - Get user's watchlist
- `POST /api/users/watchlist/{videoId}` - Add to watchlist
- `DELETE /api/users/watchlist/{videoId}` - Remove from watchlist

## Components

### VideoPlayer
- HLS video playback
- Quality selection
- Custom controls
- Fullscreen support
- Time tracking

### ContentSlider
- Horizontal scrolling
- Responsive design
- Lazy loading
- Touch support

### VideoCard
- Thumbnail display
- Video metadata
- Click handling
- Hover effects

## Database Schema

### Users Table
```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Videos Table
```sql
CREATE TABLE videos (
    id BIGSERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    url VARCHAR(255) NOT NULL,
    thumbnail_url VARCHAR(255),
    duration INTEGER,
    views BIGINT DEFAULT 0,
    genre VARCHAR(50),
    language VARCHAR(50),
    release_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Watchlist Table
```sql
CREATE TABLE watchlist (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT REFERENCES users(id),
    video_id BIGINT REFERENCES videos(id),
    added_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, video_id)
);
```

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License.
