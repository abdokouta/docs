---
icon: bullseye-arrow
---

# Quickstart

## Quick Start

Welcome to **Maginium**—your gateway to building powerful, scalable, and efficient applications. Whether you're new to web development or a seasoned professional, this guide will help you get started on your journey with Maginium. We’ll cover the essentials to ensure you’re ready to dive into building your first application. For detailed setup, see the Installation Guide and Application Structure.

***

### What is Maginium?

Maginium is a modern, flexible web framework designed to empower developers with the tools they need to create robust applications. With its expressive syntax, modular design, and an ecosystem of powerful tools, Maginium ensures you can build and maintain applications with ease.

**Key Features:**

* Modular architecture for clean and maintainable code.
* A rich set of built-in utilities for common development tasks.
* Support for caching, events, queues, and more out-of-the-box.
* Flexible integration with third-party libraries and APIs.

Maginium is built with performance and developer experience in mind, ensuring you can focus on solving your business challenges while the framework takes care of the heavy lifting.

***

### Prerequisites

Before you begin, ensure your environment meets the following requirements:

* **PHP:** Version 8.1 or higher.
* **Database:** MySQL, PostgreSQL, SQLite, or another supported database system.
* **Web Server:** Nginx, Apache, or another server configured to handle PHP.
* **Composer:** Dependency management for PHP packages.

Additionally, familiarity with basic web development concepts, PHP syntax, and terminal commands will be beneficial.

***

### Setting Up Your First Maginium Project

Once your system is ready, follow these steps to set up your first Maginium application.

#### 1. Install Maginium

Install Maginium using Composer, the PHP dependency manager:

```bash
composer create-project maginium/maginium my-app
cd my-app
```

This will create a new project in the `my-app` directory, complete with Maginium’s default structure and dependencies.

#### 2. Configure Environment

Maginium uses an `.env` file to manage environment-specific settings. Update the `.env` file in your project’s root directory to configure your database, application URL, and other necessary details:

```env
envCopy codeAPP_NAME=MaginiumApp
APP_ENV=local
APP_KEY=base64:...
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

#### 3. Serve the Application

Serve your Maginium application locally using the built-in development server:

```bash
bashCopy codephp artisan serve
```

Open your browser and navigate to `http://localhost:8000` to view your application.

### Next Steps

To deepen your understanding of Maginium, explore the next topics
