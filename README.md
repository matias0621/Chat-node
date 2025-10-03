# Chat Application

A real-time chat application built with Node.js, Express, Socket.IO, and Turso (libSQL) database.

## Features

- **Real-time messaging**: Instant message delivery using Socket.IO
- **Persistent storage**: Messages are stored in a Turso (libSQL) database
- **User authentication**: Automatic username generation using external API
- **Message recovery**: Connection state recovery for missed messages
- **Responsive design**: Clean, modern UI with dark/light mode support

## Tech Stack

- **Backend**: Node.js, Express.js
- **Real-time communication**: Socket.IO
- **Database**: Turso (libSQL) - SQLite-compatible cloud database
- **Frontend**: Vanilla HTML, CSS, JavaScript (ES6 modules)
- **Styling**: Custom CSS with system font stack

## Prerequisites

- Node.js (version 14 or higher)
- npm or yarn
- Turso account and database setup

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd Chat-node
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Set up environment variables:
   Create a `.env` file in the root directory and add your Turso database token:
   ```
   DB_TOKEN=your_turso_database_token_here
   PORT=3000
   ```

## Configuration

### Database Setup

This application uses Turso (libSQL) as the database. You'll need to:

1. Create a Turso account at [turso.tech](https://turso.tech)
2. Create a new database
3. Get your database URL and authentication token
4. Update the database URL in `server/index.js` (line 29) with your Turso database URL
5. Add your authentication token to the `.env` file

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DB_TOKEN` | Turso database authentication token | Required |
| `PORT` | Server port number | 3000 |

## Usage

1. Start the development server:
   ```bash
   npm run dev
   ```

2. Open your browser and navigate to:
   ```
   http://localhost:3000
   ```

3. Start chatting! The application will:
   - Automatically generate a username if you're a new user
   - Store your username in localStorage for future visits
   - Save all messages to the database
   - Recover missed messages when reconnecting

## Project Structure

```
Chat-node/
├── client/
│   └── index.html          # Frontend application
├── server/
│   └── index.js           # Backend server
├── package.json           # Dependencies and scripts
├── package-lock.json      # Dependency lock file
└── README.md             # This file
```

## API Endpoints

### WebSocket Events

#### Client → Server
- `chat message`: Send a new message
  ```javascript
  socket.emit('chat message', messageContent)
  ```

#### Server → Client
- `chat message`: Receive a new message
  ```javascript
  socket.on('chat message', (message, messageId, username) => {
    // Handle received message
  })
  ```

### HTTP Endpoints

- `GET /`: Serves the main chat application

## Database Schema

The application uses a single table to store messages:

```sql
CREATE TABLE messages (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    content TEXT,
    user TEXT
)
```

## Development

### Scripts

- `npm run dev`: Start the development server with auto-reload
- `npm test`: Run tests (currently not implemented)

### Key Features Implementation

1. **Username Management**: 
   - Generates random usernames using `random-data-api.com`
   - Stores username in localStorage for persistence
   - Falls back to 'antonio' if no username is provided

2. **Message Persistence**:
   - All messages are stored in the Turso database
   - Messages include content, user, and auto-generated ID

3. **Connection Recovery**:
   - Uses Socket.IO's connection state recovery
   - Fetches missed messages based on server offset
   - Prevents duplicate message delivery

4. **Real-time Updates**:
   - Broadcasts messages to all connected clients
   - Updates server offset for message tracking
   - Smooth scrolling for new messages

## Browser Support

The application uses modern JavaScript features:
- ES6 modules
- Async/await
- Fetch API
- LocalStorage

Compatible with modern browsers that support ES6+.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

ISC License

## Troubleshooting

### Common Issues

1. **Database Connection Error**:
   - Verify your Turso database URL and token
   - Check that the database is accessible
   - Ensure the `.env` file is properly configured

2. **Port Already in Use**:
   - Change the PORT in your `.env` file
   - Or kill the process using the port

3. **Username Generation Fails**:
   - Check your internet connection
   - The app will fall back to 'antonio' as username

### Debug Mode

To enable debug logging, you can modify the server to include more verbose logging or use Node.js debugging tools.

## Security Considerations

- The application uses CORS with wildcard origin for development
- Consider restricting CORS origins in production
- Database tokens should be kept secure and not committed to version control
- Consider implementing rate limiting for production use

---

Built with ❤️ using Node.js and Socket.IO
