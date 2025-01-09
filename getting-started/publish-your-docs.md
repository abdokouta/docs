---
icon: globe-pointer
---

# Installation

## Getting Started with Maginium

Welcome to **Maginium**! This guide will walk you through the steps to install and set up Maginium for your projects. Whether you're a seasoned
developer or just starting, this guide is designed to get you up and running quickly.

---

### System Requirements

Before you install Maginium, ensure your environment meets the following requirements:

- **PHP:** 8.1 or newer
- **Composer:** Latest version
- **Database:** MySQL 8.0+, PostgreSQL, SQLite, or SQL Server
- **Web Server:** Apache or Nginx
- **Extensions:**
  - OpenSSL
  - PDO
  - Mbstring
  - Tokenizer
  - JSON
  - cURL

> **Important:**\
> Make sure your server meets these requirements to avoid unexpected issues during installation.

---

### Installing Maginium

Maginium uses Composer to manage dependencies. If you don't have Composer installed,
[download and install Composer](https://getcomposer.org/download/).

#### Step 1: Create a New Project

To start a new Maginium project, run the following command in your terminal:

```bash
composer create-project maginium/framework my-project
```

This will create a new folder named `my-project` containing all the necessary files for Maginium.

---

#### Step 2: Configure Environment

Once the installation is complete, navigate to your project directory and set up the environment file:

```bash
cd my-project
cp .env.example .env
```

Edit the `.env` file to configure your database and other environment-specific settings:

```plaintext
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

> **Tip:**\
> Use a strong database password to enhance security.

---

#### Step 3: Generate Application Key

Maginium requires an application key for secure encryption. Generate it using the following command:

```bash
php maginium key:generate
```

{% hint style="info" %} This command sets a unique `APP_KEY` in your `.env` file. Ensure this key remains secret. {% endhint %}

---

#### Step 4: Serve Your Application

You can serve your application locally using the built-in development server:

```bash
php maginium serve
```

By default, your application will be accessible at `http://localhost:8000`.

---

### Additional Installation Methods

#### Using Docker

Maginium provides a pre-configured Docker setup for a streamlined development environment. To use Docker:

1. Copy the `docker-compose.yml` file provided in the project.
2. Start the containers:

   ```bash
   docker-compose up -d
   ```

3. Access your application at `http://localhost`.

{% hint style="info" %} Ensure Docker is installed and running on your system before using this method. {% endhint %}

---

### Testing Your Installation

After completing the installation, you can verify your setup by running:

```bash
php maginium test
```

This command will execute Maginium's test suite to ensure everything is working correctly.

---

### Next Steps

Congratulations! You’ve successfully installed Maginium. Here are some suggested next steps:

- Learn the Basics
- Explore Features
- Build Your First Application

---

### Troubleshooting

If you encounter issues during installation:

- Ensure your PHP version meets the requirements.
- Verify all required PHP extensions are installed and enabled.
- Check the permissions for your project directory.

Still stuck? Visit the Maginium Documentation or join our community forum for support.

---

### Conclusion

Maginium is designed to make your development journey efficient and enjoyable. By following this guide, you're now ready to dive into building
powerful applications with Maginium.

---
