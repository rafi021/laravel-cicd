## GitHub Actions CI/CD Workflow Explained

This project uses a GitHub Actions workflow for continuous integration and testing. Below is a detailed explanation of each step in the workflow defined in `.github/workflows/workflow.yml`:

### Workflow Triggers
- **on.push / on.pull_request:**  
  The workflow runs on every push or pull request to the `main` and `dev` branches.

### Job: tests

#### 1. Runner
- **runs-on: ubuntu-latest**  
  The workflow uses the latest Ubuntu runner provided by GitHub.

#### 2. MySQL Service
- **services.mysql**  
  - Uses the `mysql:8.0` Docker image.
  - Exposes port `3306`.
  - Sets environment variables for the root password and a test database.
  - Includes a health check to ensure MySQL is ready before running tests.

#### 3. Steps

- **actions/checkout@v4**  
  Checks out your repository code so the workflow can access it.

- **Setup PHP with PECL extension**  
  Uses `shivammathur/setup-php@v2` to install PHP 8.4 and the required extensions (`imagick`, `swoole`).

- **Copy .env file**  
  Copies `.env.ci` to `.env` to set up environment variables for the CI environment.

- **Install dependencies**  
  Installs Composer dependencies with optimized flags for CI (quiet, no interaction, no scripts, no progress).

- **Generate application key**  
  Runs `php artisan key:generate` to set the Laravel application key, which is required for encryption and sessions.

- **Directory permissions**  
  Sets the correct ownership and permissions for the `storage` and `bootstrap/cache` directories, which Laravel needs to write cache and logs.

- **phpstan**  
  Runs static analysis using PHPStan at level 5 to catch potential bugs and code issues.

- **phpinsights**  
  Runs PHP Insights to analyze code quality, complexity, architecture, and style. The workflow enforces minimum thresholds for each metric.

- **Run tests**  
  Executes the Laravel test suite using `php artisan test`.

---

This workflow ensures that your code is automatically checked for quality, correctness, and passes all tests before being merged or deployed.
