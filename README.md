# Number Guessing Game

A PostgreSQL-backed command-line number guessing game built with Bash scripting.

## Overview

This project consists of a Bash script that implements an interactive number guessing game, backed by a PostgreSQL database to track player statistics and game history. Players guess a random number between 1 and 1000, with the game providing hints and recording their performance.

## Features

- **Player Recognition**: System recognizes returning players and displays their statistics
- **Game Statistics**: Tracks number of games played and best performance (fewest guesses)
- **Input Validation**: Validates user input to ensure only integers are accepted
- **Game History**: Stores all game results in the database
- **Helpful Hints**: Provides "higher" or "lower" feedback after each guess

## Database Schema

### Tables

1. **players**
   - `user_id` (integer, primary key): Unique identifier for each player
   - `username` (varchar(30), unique): Player's username

2. **games**
   - `game_id` (integer, primary key): Unique identifier for each game
   - `user_id` (integer, foreign key): References players.user_id
   - `secret_number` (integer): The target number for the game
   - `number_of_guesses` (integer): Number of guesses taken to win

### Relationships
- One player can have many games (one-to-many relationship)
- Foreign key constraint ensures referential integrity

## Installation & Setup

### Prerequisites
- PostgreSQL 12.9 or compatible
- Bash shell
- `psql` command-line tool

### Database Setup

1. Run the SQL script to create the database and tables:
```bash
psql -U postgres -f number_guess.sql
```

2. Alternatively, you can run the SQL commands directly in psql.

### File Permissions
Make the Bash script executable:
```bash
chmod +x number_guess.sh
```

## Usage

Run the game script:
```bash
./number_guess.sh
```

### Game Flow

1. **Username Prompt**: Enter your username (new or returning player)
2. **Personalized Greeting**:
   - New players: Welcome message
   - Returning players: Statistics showing games played and best score
3. **Gameplay**: Guess numbers between 1 and 1000 with hints
4. **Victory**: Display number of guesses taken and secret number
5. **Data Storage**: Game result automatically saved to database

## Code Structure

### Bash Script (`number_guess.sh`)

- **Database Connection**: Uses `psql` with specific connection parameters
- **Player Management**: Checks for existing players or creates new ones
- **Game Logic**: Random number generation, input validation, and hint system
- **Statistics Tracking**: Records and displays player performance

### Key Functions
- Random number generation: `SECRET_NUMBER=$(( RANDOM % 1000 + 1 ))`
- Input validation using regex: `[[ ! $USER_GUESS =~ ^[0-9]+$ ]]`
- Database queries for player lookup and statistics

## Database Configuration

The script connects to PostgreSQL with:
- Username: `freecodecamp`
- Database: `number_guess`
- Connection options: `-t --no-align -c` for clean output

## Game Rules

1. The secret number is randomly generated between 1 and 1000
2. Players guess integers until they find the correct number
3. After each guess, the game indicates if the secret number is higher or lower
4. All guesses are counted toward the final score
5. Invalid inputs (non-integers) are rejected but still count as guesses

## Example Session

```
Enter your username:
alice

Welcome, alice! It looks like this is your first time here.

Guess the secret number between 1 and 1000:
500
It's higher than that, guess again:
750
It's lower than that, guess again:
625
You guessed it in 3 tries. The secret number was 625. Nice job!
```

## Error Handling

- Handles non-integer inputs with appropriate error messages
- Manages database connection failures gracefully
- Prevents SQL injection through proper variable usage

## Dependencies

- PostgreSQL database server
- Bash 4.0 or higher
- Standard Unix utilities

## Extending the Game

Potential enhancements:
- Difficulty levels (different number ranges)
- Time-based scoring
- Leaderboard functionality
- Multiple player game modes
- Graphical interface option

## Troubleshooting

1. **Database connection issues**: Verify PostgreSQL is running and credentials are correct
2. **Permission errors**: Check file permissions and database user privileges
3. **Script not executing**: Ensure shebang line is correct and file is executable

## License

This project is part of the freeCodeCamp curriculum and is intended for educational purposes.
