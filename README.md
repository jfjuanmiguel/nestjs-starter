# NestJS Starter Project

This NestJS starter project serves as a robust foundation for building scalable and maintainable backend applications. It incorporates best practices, essential modules, and configurations to jumpstart your development process.

## Features

- **Enhanced TypeScript Configuration**: Improved `tsconfig.json` for stricter type checking and better developer experience.
- **Environment Configuration**: Utilizes `ConfigModule` for managing environment variables, including Jest setup.
- **Consistent HTTP Responses**: Enforces a standardized structure for all HTTP responses.
- **Basic HTTP Security**: Implements essential security measures to protect your application.
- **Request Validation**: Incorporates whitelisted validation for incoming requests.
- **Advanced Logging**: Utilizes Winston for comprehensive logging capabilities.
- **Database Setup**: Docker Compose configuration for PostgreSQL and Redis.
- **ORM Integration**: Prisma setup for efficient database interactions.
- **Caching Solution**: Redis integration with a CacheService for improved performance.
- **Testing Framework**: Jest configuration with environment variable support.
- **CI Pipeline**: GitHub Actions setup for continuous integration.

## Core Module

The Core Module (`CoreModule`) serves as the central hub for essential application-wide services and configurations. It includes:

- Global exception filters
- Interceptors for response transformation
- Logging service
- Caching service
- Database module

When extending core functionality, consider adding services or providers to this module if they are required application-wide.

## Logging

This starter uses Winston for advanced logging capabilities. The `LoggerService` in the Core Module provides:

- Customizable log formats for different environments
- Log levels (error, warn, info, debug, verbose)
- Context-based logging
- Metadata support for detailed log entries

To use the logger in your services or controllers, inject the `LoggerService` and utilize its methods for consistent logging across your application.

## Getting Started

1. Go to the NestJS Starter Github repo:
2. Press the "Use this template" button to create a new repository.
3. Follow the steps to create a new Github repo from the template.
4. Clone the repository:
   ```
   git clone https://github.com/jfjuanmiguel/nestjs-starter.git
   ```

## Local Development

1. Install dependencies:
   ```
   pnpm install
   ```
2. Set up your environment variables:
   Copy `.env.example` to `.env` and fill in the required values.
3. Start the development server:
   ```
   pnpm start:dev
   ```
   The NestJS Starter project has 2 Docker Compose files: In both files, you need to update the name of the containers and networks where it says `# Needs updating`.

For example, here's an updated `docker-compose.yml` file for a project called `URL Shortener`:

```
version: '3.8'

services:
  postgres_url_shortener: # Updated
    image: postgres:alpine
    container_name: postgres_url_shortener # Updated
    restart: always
    env_file:
      - .env
    environment:
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    ports:
      - '5432:5432'
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis_url_shortener: # Updated
    image: redis:alpine
    container_name: redis_url_shortener # Updated
    ports:
      - '6379:6379'
    volumes:
      - redis_data:/data

networks:
  default:
    name: url_shortener # Updated

volumes:
  postgres_data:
  redis_data:
```

Make sure you remember to also update the `docker-compose-test.yml` file!

This repo comes with a default `User` model out of the box defined in the `/src/database/schema.prisma` file:

```
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

Before you can run the local server, you need to apply this migration to your local Postgres database.

Make sure on your local machine you don't have any existing Docker containers running that would cause a conflict.

Then spin up the local Postgres and Redis containers:

```
pnpm docker:start
```

Then run this script to apply the migration to your local Postgres database:

```
pnpm db:migrate:dev
```

And you're ready to go!

You can now start the local server:

```
pnpm start:dev
```
