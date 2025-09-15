
# pg_service

`pg_service` is a simple utility for managing PostgreSQL connection services via the `~/.pg_service.conf` file. It provides a streamlined interface for listing, retrieving, and modifying PostgreSQL connection entries, making it easier to work with multiple database configurations.

## Features
- **Set a service**: Add or update PostgreSQL service configurations.
- **Edit configuration**: Open `~/.pg_service.conf` in your default text editor.
- **List services**: View all available PostgreSQL service names.
- **Get connection string**: Retrieve the full PostgreSQL connection string for a service.
- **Shell completion**: Provides autocompletion support for `zsh`.

## Installation
0. Install dependencies:
    - `python3`
    - `fzf` (for interactive selection in `get` command, optional)
    - `psql` (obviously)
1. Clone this repository:
   ```sh
   git clone <repository_url>
   cd pg-service
   ```
2. Ensure the scripts are executable:
   ```sh
   chmod +x pg_service bin/*
   ```
3. Add the `pg_service` script to your `PATH`:
   ```sh
   export PATH="<wherever you put the service>:$PATH"
   alias pgs=pg_service
   source <(pg_service completion zsh)
   ```

## Usage
```sh
pgs set mydb postgres://user:pass@localhost:5433/mydb
pgs get mydb
# postgres://user:pass@localhost:5433/mydb
psql service=mydb
```

## Commands
-  `pgs set service_name connection_string [--dry-run]`
   -  Add or update a PostgreSQL service configuration. With `--dry-run`, just print the configuration that would be generated. For example:
        ```sh
        pgs set my-db-local postgresql://user:pass@localhost:5432/mydatabase
        ```

- `pgs ls`
  - Prints a list of all defined PostgreSQL services.

- `pgs get [service_name]`
  - Retrieve the connection string for a service.  If no service name is provided, an interactive `fzf` selection is used.

- `pgs edit`
  - Open `~/.pg_service.conf` in `vim` for manual modifications.

- `pgs completion` – Generate a `zsh` autocompletion script.
    - To enable autocompletion for `pg_service` in `zsh`, add the following to your `.zshrc`:
        ```sh
        source <(pg_service completion zsh)
        ```
    - Then, you'll get service name autocompletion in these contexts:
        ```sh
        psql service=<TAB>
        or
        pgs subcommand <TAB>
        ```

## Using Configured Services with `psql`
Once services are configured using `pg_service`, they can be used directly with `psql` by specifying the service name:
```sh
psql service=<service_name>
```
For example:
```sh
psql service=mydb
```
If you have set up completion, the name can be tab completed.  This automatically connects to the PostgreSQL database using the credentials and settings stored under `mydb` in `~/.pg_service.conf`.

